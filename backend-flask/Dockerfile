# using multi-staging to build the app

# Stage 1: Build the application
FROM python:3.10-slim-buster AS builder

WORKDIR /backend-flask

COPY requirements.txt requirements.txt
RUN pip3 install --user -r requirements.txt

COPY . .

# Stage 2: Copy the application into a production-ready image
FROM python:3.10-slim-buster

WORKDIR /backend-flask

COPY --from=builder /root/.local /root/.local
COPY --from=builder /backend-flask .

ENV PATH=/root/.local/bin:$PATH
ENV FLASK_ENV=development

EXPOSE ${PORT}

COPY entrypoint.sh .

# Run the dockerfile CMD as an external script
CMD ["./entrypoint.sh"]