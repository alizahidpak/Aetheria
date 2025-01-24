FROM node:22-alpine AS builder
WORKDIR /app

# install pnpm globally (in container)
RUN npm install -g pnpm

# copy package.json and pnpm-lock.yaml files
COPY package*.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile

# copy source code
COPY . ./

# build application
RUN pnpm build

FROM node:22-alpine AS runner
WORKDIR /app

# install pnpm globally (in runner image)
RUN npm install -g pnpm

# copy required dependencies (only production required)
COPY package*.json pnpm-lock.yaml ./
COPY --from=builder /app/node_modules ./.pnpm-store
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public

# start application
CMD ["pnpm", "start"]