FROM node:16-alpine as builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . ./

RUN npm run build


FROM node:16-alpine as runner

WORKDIR /app

COPY package*.json ./

RUN npm ci --omit=dev && npm i -g pm2

COPY --from=builder /app/ecosystem.config.js /app/

COPY --from=builder /app/dist ./dist

ENTRYPOINT [ "pm2-runtime", "start", "ecosystem.config.js" ]