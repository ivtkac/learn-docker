---
created: 2025-11-22
title: Dockerfile
tags:
  - docker
aliases:
doc: https://docs.docker.com/reference/dockerfile/
---
> [!tldr]
> **Dockerfile** — текстовий файл, який містить команди, які користувач може викликати у командному рядку для створення образу.

> [!example] Recipe
> 1. Start with OS
> 2. Install language runtime
> 3. Install application deps
> 4. Set up execution environment
> 5. Run application

## Bad dockerfile

```dockerfile
FROM ubuntu

RUN apt update && apt install nodejs npm -y

COPY . .

RUN npm install

CMD ["npm", "run", "dev"]

```

## Good docker file

```dockerfile
FROM node:19.6-alpine

WORKDIR /usr/src/app

ENV NODE_ENV=production

COPY package*.json ./

RUN --mount=type=cache,target=/usr/src/app/.npm \
  npm set cache /usr/src/app/.npm && \
  npm ci --only=production

USER node

COPY --chown=node:node ./src .

EXPOSE 3000

CMD ["node", "index.js"]
```