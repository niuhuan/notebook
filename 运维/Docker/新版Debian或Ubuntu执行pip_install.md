
使用venv

```dockerfile
FROM ubuntu:rolling

RUN apt update && apt install -y python3 python3-pip python3-venv zip unzip 
RUN python3 -m venv /opt/venv

# Activate the virtualenv:
ENV PATH="/opt/venv/bin:$PATH"

# Now we can install python packages without affecting the system python3
RUN pip3 install telethon
```