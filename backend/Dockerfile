FROM rust:latest as builder

# Set the working directory in the container
WORKDIR /app

# Copy the backend and frontend files into the container
COPY . . 

# Install any dependencies and build the project
RUN apt-get update && apt-get install -y \
    build-essential \
    libssl-dev \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

RUN rustup update
RUN cargo build --release -p backend 
# Set up the final runtime image

FROM debian:bullseye-slim

# Install any runtime dependencies needed for the app (e.g., for serving frontend files)
RUN apt-get update && apt-get install -y \
    libssl1.1 \
    && rm -rf /var/lib/apt/lists/*

# Copy the built binary and frontend files from the builder image
WORKDIR /app
COPY --from=builder /app/target/release/backend ./backend
COPY --from=builder /app/frontend/dist ./frontend/dist

# Run the backend binary
CMD ["./backend/backend"]

