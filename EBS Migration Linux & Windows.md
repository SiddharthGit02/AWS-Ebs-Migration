EBS Migration Linux & Windows

Create Ubuntu AWS Ec2
Create 2 extra volumes and attach

Install docker :
```
sudo apt update
sudo apt install docker.io docker-compose-v2 -y
sudo usermod -aG docker ubuntu
```

Create directories :
```
sudo mkfs.ext4 /dev/nvme1n1
sudo mkfs.ext4 /dev/nvme1n1
sudo mount /dev/nvme1n1 /mnt/app_data
```

```
mkdir -p /mnt/app_data/compose-test
mkdir -p /mnt/app_data/redis_storage
```

Start application
sudo docker compose up -d
http://<YOUR_EC2_PUBLIC_IP>:5000

```
sudo mkfs.ext4 /dev/nvme2n1
sudo mkdir -p /mnt/target_drive
sudo mount /dev/nvme2n1 /mnt/target_drive
```

Copy contents using rsync

`sudo rsync -aHAXxv --numeric-ids /mnt/app_data/ /mnt/target_drive/`

Swap drives 
```
sudo umount /mnt/app_data
sudo umount /mnt/target_drive
sudo mount /dev/nvme2n1 /mnt/app_data  # Mount the NEW drive to the OLD location
```

To make mount permanent, add in /etc/fstab
`UUID=12345abc-6789-def0-1234-56789abcdef0  /data  ext4  defaults,nofail  0  2`


Windows
Create Windows 2016 base Ec2
with 2 extra volumes

Download Google chrome, DB sqlite
create Tables with execute sql

-- Create a sample table
```
CREATE TABLE server_logs (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    event_name TEXT NOT NULL,
    status TEXT NOT NULL,
    logged_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

-- Insert sample data
```
INSERT INTO server_logs (event_name, status) VALUES 
('Volume_Initialize', 'Success'),
('Data_Population', 'In_Progress'),
('Robocopy_Sync', 'Pending'),
('Drive_Letter_Swap', 'Pending'),
('Application_Resume', 'Pending');
```

-- Verify the data was written
`SELECT * FROM server_logs;`


In windows for copy : 
use robocopy - It copies with everything like file permissions, attributes
`robocopy D:\ E:\ /E /COPYALL /R:3 /W:3`