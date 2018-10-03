Quick instructions.

1. Build the container:

    docker build -t phytoplankton_container .

2. Run the container:

    docker run -ti -p 5000:5000 phytoplankton_container deepaas-run --listen-ip 0.0.0.0
