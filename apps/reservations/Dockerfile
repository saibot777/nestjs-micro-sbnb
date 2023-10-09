FROM node:18.17.0-alpine as development

# Create app directory
RUN mkdir -p /users/src/app
WORKDIR /users/src/app

COPY package.json ./
COPY pnpm-lock.yaml ./

RUN npm install -g pnpm

RUN npm install -g yarn --force

RUN yarn add @nestjs/cli --global

RUN pnpm install

COPY . .

RUN pnpm run build

FROM node:18.17.0-alpine as production

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /users/src/app

COPY package.json ./
COPY pnpm-lock.yaml ./

RUN npm install -g pnpm

RUN pnpm install --prod

COPY --from=development /users/src/app/dist ./dist

CMD [ "pnpm", "run", "start:dev", "reservations" ]
