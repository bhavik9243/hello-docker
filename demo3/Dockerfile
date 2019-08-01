FROM python:alpine

WORKDIR /

COPY Main.py start.sh requirements.txt /
RUN chmod +x /start.sh
RUN pip install -r requirements.txt

ENTRYPOINT ["/start.sh"]
