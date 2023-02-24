# using multi-staging to build the app
# Stage 1: Build the application
FROM node:16.18 AS build

WORKDIR /frontend-react-js

COPY . /frontend-react-js

RUN npm install
RUN npm run build

# Stage 2: Copy the built application into a production-ready image
FROM node:16.18-slim

ENV PORT=3000

WORKDIR /frontend-react-js

COPY --from=build /frontend-react-js/build /frontend-react-js
COPY --from=build /frontend-react-js/package*.json /frontend-react-js/

RUN npm install

EXPOSE ${PORT}
COPY start.sh .
# Run the dockerfile CMD as an external script
CMD ["./start.sh"]


