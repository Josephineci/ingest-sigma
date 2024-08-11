# All images are now persisted in shared services registry
FROM 681030429768.dkr.ecr.eu-west-2.amazonaws.com/rust_base_image:latest as builder
WORKDIR /usr/src/sigma

# Don't remove these, they're needed for SSH during cargo build
ARG GH_SSH
ENV GH_SSH=${GH_SSH}
    
COPY /src ./src/
COPY ["Cargo.toml", "./"]
# <https://github.com/team-sigma-ai/sigma-rust-images/blob/main/run_cargo_build.sh>
RUN run_cargo_build.sh

#Setting up the deployment environment
FROM 681030429768.dkr.ecr.eu-west-2.amazonaws.com/rust_bookworm_slim_image:latest

# Create a non-root user with a non-login shell
RUN mkdir -p /home/sigmauser &&\
    groupadd -r sigmauser &&\
    useradd -r -g sigmauser -d /home/sigmauser -s /sbin/nologin -c "Docker image user" sigmauser

WORKDIR /usr/src/sigma

# Copy the binary from the builder stage
COPY --from=builder /usr/src/sigma/target/release/ingest-rs .
COPY ./config/default.yml ./config/default.yml

# Change the ownership to the non-root user
RUN chown -R sigmauser:sigmauser /usr/src/sigma

# Use the non-root user to run the application
USER sigmauser

# Ensure the binary is executable
RUN chmod +x ./ingest-rs

CMD ["./ingest-rs"]

