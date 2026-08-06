FROM node:26-alpine

RUN apk add --no-cache curl tini

WORKDIR /usr/local/app

COPY package.json .
RUN npm install --omit=dev --no-cache

COPY . .
RUN chown -R node:node /usr/local/app
USER node

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:80/health || exit 1

CMD ["node", "server.js"]