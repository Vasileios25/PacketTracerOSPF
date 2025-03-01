
# OSPF-SUBNETTING

## Description
The project is a Simulation of Designed Network Topology to demonstrate ospf,subnetting,dhcp relay,dns relay,communication between different subnets with routers acting as default gateway and use of serial links:

### Features:
- Used the ip 192.168.1.0/24 and the task was to divide into 4 subnets.
- 2 to the power of n equals 4 so n=2, means borrow(steal!) 2 bits from the host portion making it /26 .
- So 4 subnets 192.168.1.0, 192.168.1.64, 192.168.1.128, 192.168.1.192.
- The first and the third subnet are assign at DHCP pool for the machines that are at behind the  Layer 2 switches
- The second is assign at serial link connections between default gateway routers
- The last one is asign at DNS server
- The DHCP server is in the same subnet, the 192.168.1.0
- The DNS serves a very simple html 


---

## **1. Initial Setup**

- **Selection of routers**:
  - use 2 `4321` routers and connect them with serial link.
  - Serial link here represent a P2P connection in order to simulate real-world WAN links.
  - Put modules `NIM-2T` and `NIM-ES2-4` that provides 2x Serial Ports and the 2 provides four Gigabit Ethernet switching ports, enabling integrated switching capabilities within the router.Bring first the router down, put them and
    bring them up again.
  - Bring the serial port up. First we should assign the corresponding ip's.For router0, `enable`, `config t`, `int Serial0/1/0`, `ip address 192.168.1.65 255.255.225.192`, `no shut`. The same for the router1.
  - In Packet Tracer (and real Cisco routers), when using a serial cable, one router acts as the DCE (Data Communication Equipment) and the other as the DTE (Data Terminal Equipment).
  - The DCE side must set a clock rate, or the link will remain down.The clock rate controls the speed of data transmission over the link.If there is no clock rate, the routers don't know at what speed to transmit data, 
    so the interface stays down.
  - The router0 is the DCE and the router1 the DTE. For the DCE part we go to config mode, choose the interface and put the command `clock rate <rate number in bits per second (bps)>`. For the DTE part we don't do nothing.
  - Use the `show running-config | section interface Serial` command in config mode in order to check the configuration of the DCE,DTE.

---

- **Selection of switches**:
  - use 2 `pt-switch`
  - A cursor object is created to execute SQL queries.

---

## **3. Retrieving Previous Data from Database**

- The script fetches the last entry from the `Calculator` table using the following query:
  ```sql
  SELECT * FROM Calculator ORDER BY id DESC LIMIT 1;
  ```
- If no previous data exists, it initializes the statistics to `[0, 0, 0, 0, 0, 0]` (representing counts for each operation).

---


## Docker Compose File Explanation

This Docker Compose (`docker-compose.yml`) file sets up a multi-container environment with:
1. **PostgreSQL Database** (`postgres-db`)
2. **Vault Service** (`vault`) for secrets management
3. **Python Application** (`python-app`), which interacts with the database

---

## **Services**

### **1. PostgreSQL Database (`postgres-db`)**

This service runs a **PostgreSQL 13** database inside a Docker container.

- **Image**: Uses the official PostgreSQL 13 image.
- **Container Name**: `postgres-container`
- **Exposed Ports**: Internally exposes port **5432** (default PostgreSQL port).
- **Environment Variables**: Loads database credentials from `.env` variables.
- **Volumes**:
  - `init_db.sql`: Runs an SQL script at startup to create the **Calculator** table:
    ```sql
    CREATE TABLE IF NOT EXISTS Calculator (
      id integer,
      addition integer,
      substraction integer,
      multiplication integer,
      division integer,
      square integer
    );
    ```
  - `my_volume`: Stores database data persistently.
- **Restart Policy**: Always restarts the container if it stops.
- **Network**: Connects to `my-network` for communication with other services.

---

### **2. Vault (`vault`)**

This service runs **HashiCorp Vault**, used for securely storing and managing secrets.

- **Build Context**: Uses a custom `Dockerfile.vault` to build the container.
- **Container Name**: `vault`
- **Network Mode**: Uses the **host network** (directly binds to the host’s network stack).
- **Environment Variables**:
  - `VAULT_ADDR`: Specifies the Vault server address.
  - `VAULT_TOKEN`: Uses an environment variable for authentication.
- **Volumes**:
  - Mounts the Vault configuration file (`vault-config.hcl`).
  - Mounts a `.env` file to store passwords.
  - `apply-policies.sh`: Script for applying security policies.
  - *(Optional)* `kv-policy.hcl`: Can be added for Vault policy configuration.

---

### **3. Python Application (`python-app`)**

This service runs a **Python script** that connects to the database and performs calculations.

- **Restart Policy**: Always restarts the app if it crashes.
- **Build Context**: Uses the default `Dockerfile` to build the Python environment.
- **Container Name**: `python-app-container`
- **Dependencies**: Waits for `postgres-db` to be ready before starting.
- **Command & Entry Point**:
  - Uses `wait-for-it.sh` to ensure the database is up before running `Calculator.py`.
  - Executes the Python script (`Calculator.py`) after confirming PostgreSQL is available.
- **Network**: Connects to `my-network` for database communication.

---

## **Networks**
- `my-network`: A bridge network for communication between the `postgres-db` and `python-app`.

---

## **Volumes**
- `my_volume`: Stores PostgreSQL data persistently to prevent data loss when the container restarts.

---

## wait-for-it

This script is designed to wait for a PostgreSQL server to become available before executing a command. It's commonly used in Docker container setups, where the Python application or other services may depend on a PostgreSQL database being up and ready.

---

### **1.Assign Variables:**
```bash
host="$1"
port="$2"
shift 2
cmd="$@"
```

### **1.Assign Variables:**


## Getting Started

### Dependencies

* Python 3.x
* Docker
* Docker Compose

### Installing

1. Clone the repository:
   ```bash
   git clone https://your-repo-url.git
   cd Calculator_Application
   ```

2. Create a `.env` file for environment variables (database credentials) if not using Vault yet.

3. Build and start the Docker containers:
   ```bash
   docker-compose up --build
   ```

4. Open the Python app container and interact with the calculator.

### Executing program

Run the application using:
```bash
docker-compose up
```

Once the app is running, run in other terminal:
```bash
docker container ls
```

connect to python app to run the python script and the same for the other containers:
```bash
docker exec -t <container> /bin/bash
```

to connect to to postgresql run the following:
```bash
psql -U <name>
```

List available databases:
```bash
\l
```

Switch connection to a new database:
```bash
\c dbname
```

List available tables:
```bash
\dt
```

Describe a table such as a column, type, modifiers of columns, etc.:
```bash
\d table_name
```

## Help

For common issues, check the logs or run the following command for help:
```bash
docker-compose logs
```

## Authors

Contributors names and contact info

ex. Vasilhs S.  


## Version History



## License


## Acknowledgments


