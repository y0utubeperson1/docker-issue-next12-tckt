# Base stage
FROM node:20-bullseye-slim AS base

WORKDIR /app

COPY package*.json ./

# Development stage
FROM base AS development

RUN npm install

COPY . .

# disable telemetry for better performance
ENV NEXT_TELEMETRY_DISABLED=1

EXPOSE 3000

CMD ["npm", "run", "dev"]

# Production stage
FROM base AS production

COPY --from=build /app/package*.json ./
COPY --from=build /app/.next ./.next

RUN npm ci --only=production && \
    npm cache clean --force && \
    rm -rf /tmp/*

EXPOSE 3000

CMD ["npm", "start"]
