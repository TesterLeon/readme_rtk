# RTK Raspberry Pi System

A Real-Time Kinematic (RTK) positioning system implemented on a Raspberry Pi, featuring a Flask web interface for data collection and NTRIP client functionality.

## Quick Start Guide

### Hardware Setup

1. **Power Up Raspberry Pi**
   - Connect power supply
   - Verify green LED indicator is on

2. **Connect Antennas**
   - Antenna 2 → Black box
   - Antenna 1 → White box
   - Black USB cable: Black box → Lower gray USB port on Pi
   - Red USB cable: White box → Upper gray USB port on Pi

### System Access

#### Option 1: Direct Connection
- Connect monitor, keyboard, and mouse directly
- Connect to internet if needed

#### Option 2: Remote Access (VNC)
1. Find Pi's IP address:
   ```bash
   ping raspberrypi.local
   ```
2. Use RealVNC Viewer
3. Login credentials:
   - Username: `leon`
   - Password: `password`

### Starting the System

1. Open terminal
2. Execute startup script:
   ```bash
   bash startme.sh
   ```

### Web Interface Access
- URL: `rtk.schard.in`
- Login credentials:
   - Username: `mike`
   - Password: `meingottwalter`

## Technical Details

### Flask Application Structure

The system uses a Flask web application (`webapp.py`) with the following components:

```python
from flask import Flask, render_template, request, redirect, url_for, make_response
import subprocess
import threading
```

### Key Features

1. **NTRIP Client Integration**
   - Connects to NTRIP caster for RTK corrections
   - Configurable connection parameters
   - Supports dual antenna setup

2. **Data Collection**
   - Threaded measurement process
   - Automatic data pipe handling
   - Real-time NMEA processing
   - Local data storage by default

3. **Optional Cloud Storage**
   - Optional Cloudflare R2 integration for data storage
   - If enabled, provides automatic file upload after measurement
   - Unique identifiers for each dataset
   - System works fully without cloud storage

### Configuration

#### NTRIP Settings
Default configuration in `webapp.py`:
```python
"ntrip://gast:gast@sapos.geonord-od.de:2101/RTCM4G"
```

To modify NTRIP settings:
1. Navigate to `rtklib` directory
2. Edit `webapp.py`
3. Update NTRIP string format:
   ```python
   "ntrip://Username:Password@URL:Port/Mountpoint"
   ```

#### Optional Cloud Storage Configuration
If you want to enable cloud storage:
1. Install required package:
   ```bash
   pip install boto3
   ```
2. Configure storage client in `webapp.py`:
   ```python
   s3_client = boto3.client(
       's3',
       endpoint_url='your_endpoint_url',
       aws_access_key_id='your_access_key',
       aws_secret_access_key='your_secret_key'
   )
   BUCKET_NAME = 'your_bucket_name'
   ```

### Measurement Process

1. **Initialization**
   - Sets up USB permissions
   - Creates data pipes
   - Starts NTRIP clients

2. **Data Collection**
   - Runs for 60 seconds by default
   - Processes NMEA data from both antennas
   - Stores data in JSON format locally

3. **Data Storage**
   - Primary: Saves data locally in JSON format
   - Optional: Uploads data to cloud storage if configured
   - Local files are retained unless cloud upload is enabled

## Troubleshooting

### Common Issues
1. **USB Connection Problems**
   - Verify USB permissions are set correctly
   - Check cable connections
   - Ensure proper USB port assignment

2. **NTRIP Connection Issues**
   - Verify internet connectivity
   - Check NTRIP credentials
   - Confirm mountpoint availability

### Support Contact
For urgent assistance: 017641834567

## Development and Maintenance

### Running the Application
```bash
python webapp.py
```

The application runs on:
- Host: `0.0.0.0`
- Port: `80`

### File Structure
```
rtklib/
├── webapp.py
├── process_nmea.py
├── process_nmea2.py
├── str2str
└── templates/
    ├── index.html
    ├── measurement.html
    └── results.html
```

## License
This project is proprietary and confidential. All rights reserved.
