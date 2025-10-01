FROM node:22-slim AS builder

RUN apt-get update && \
    apt-get install -y --no-install-recommends curl tini && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app/result

COPY package*.json ./

RUN npm install && npm cache clean --force

COPY . .

ENV PORT=80
EXPOSE 80

ENTRYPOINT ["/usr/bin/tini", "--"]

CMD ["node", "server.js"]