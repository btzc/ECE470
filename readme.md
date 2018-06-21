# Wedding Seat Problem

## Running in Docker

Pull the Docker image (first time only)
```bash
docker pull scrussell24/deap
```

Run the Docker container (from the project directory)
```bash
docker run -p 8888:8888 -v `pwd`:/deap scrussell24/deap
```