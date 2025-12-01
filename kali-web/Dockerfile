FROM kasmweb/core-kali-rolling:1.18.0-rolling-daily

USER root

RUN echo "kasm-user ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Replace Kasm repo with full Kali repo
RUN echo "deb http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware" \
    > /etc/apt/sources.list \
    && apt update \
    && apt install -y kali-linux-large iputils-ping

USER kasm-user

RUN python3 -m venv /home/kasm-user/.venv/sshmitm \
    && /home/kasm-user/.venv/sshmitm/bin/pip install --upgrade pip \
    && /home/kasm-user/.venv/sshmitm/bin/pip install "ssh-mitm[production]"

