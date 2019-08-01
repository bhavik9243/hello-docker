FROM python:alpine

WORKDIR /

COPY Main.py start.sh /
RUN chmod +x /start.sh

ENTRYPOINT ["/start.sh"]
CMD ["hello","world"]
