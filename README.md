# crewstand.compose

## Overview

1. **Backend Service**: Handles the control logic, data processing, and communication with the sensors and actuators.
2. **Frontend Web Application**: Provides a user-friendly interface for real-time data visualization and system control.
3. **Grafana**: A visualization and analytics software for creating custom dashboards based on data from InfluxDB.
4. **InfluxDB**: A time series database that stores sensor and actuator data and other time-related information.
5. **PID-Control Service**: Implements PID (Proportional-Integral-Derivative) control for flow rate.
6. **Sensor-Gateway**: A service interfacing with the flow sensors to collect real-time data.
7. **Classifier**: The Classifier service, it uses a LightGBM model to classify data based on inputs from flow sensor and event definition.


## Requirements

To run the CrewStand system, you need:

1. **Docker**: Install Docker for your operating system (Windows/Mac/Linux).
2. **Docker Compose**: Included within the Docker installation on most systems.
3. **Network Access**: Ensure access to required network ports as specified in the `docker-compose.yml`.

## Getting Started

### 1. Configuration

#### For InfluxDB:

Copy `.env.example` to `.env` and fill in your InfluxDB details.
Edit `.env` file to contain your actual InfluxDB bucket, organization, and token:

```
INFLUXDB_BUCKET=<your_bucket>
INFLUXDB_ORG=<your_org>
INFLUXDB_TOKEN=<your_token>
```

Persistent volumes are used for InfluxDB configuration and data. Ensure that these paths are correctly configured in your `.env` file, then reference them in `docker-compose.yml` to match your specific environment. Add or adjust the following environment variables in your `.env` file to specify the paths of your volumes:
```
INFLUX_DATA_VOLUME=<your_data_volume_path>
INFLUX_CONFIG_VOLUME=<your_config_volume_path>
```


### 3. Running the System

Use `docker-compose` to start all components.

```bash
docker-compose up -d
```

### 4. Accessing Components

After successful deployment:

- **Frontend**: Access the web interface at `http://localhost`.
- **Grafana**: View dashboards at `http://localhost:3000`. Credentials will depend on your Grafana configuration.
- **Backend**: API is running at `http://localhost:5000`.
- **InfluxDB**: Available at `http://localhost:8086` (details depend on your configuration).

## Troubleshooting

- **Ports in Use**: If you encounter issues with port conflicts, ensure that the specified ports (80, 5000, 8086, 3000) are not being used by other services.
- **Docker Errors**: If Docker encounters issues, ensure your Docker engine is running and Docker Compose is installed.
- **Service Connectivity**: If services fail to connect, ensure networking is correctly set up and that the `depends_on` configuration in `docker-compose.yml` initializes services in the appropriate order.

## Development Notes

### Services Overview

- **Backend Service**: Routes for sensor data collection, control actions, and communication with other microservices.
- **Frontend**: A React-based web application.
- **Grafana**: Customizable dashboards, primarily depends on InfluxDB data source.
- **PID-Control Service**: Implements control logic based on PID algorithm for precise environmental control.
- **Sensor-Gateway**: Mocks or connects to real sensors, formats data, and sends it to the backend service.
- **Classifier**: Executes a LightGBM machine learning model for classification. It interacts with the backend and relies on InfluxDB for fetching and storing classification results.