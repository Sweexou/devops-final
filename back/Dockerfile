FROM node:22-alpine AS builder

ENV DATABASE_URL="file:./file.db"
ENV JWT_SECRET="your_secret"
ENV BCRYPT_SALT_ROUNDS=10

ARG DEFAULT_ADMIN_PASSWORD
ENV DEFAULT_ADMIN_PASSWORD=${DEFAULT_ADMIN_PASSWORD}

WORKDIR /app

COPY package*.json ./

RUN npm install  

RUN apk update \
  && apk add --no-cache openssl\
  && rm -rf /var/lib/apt/lists/* \
  && rm -rf /var/cache/apk/*

COPY . .

RUN npx prisma generate 
RUN npx prisma db push
RUN npx prisma db seed

# Production stage
FROM builder
EXPOSE 3000

CMD ["node", "./server.js"]