# base image
FROM node:12.2.0-alpine as build-stage

# set working directory
WORKDIR /app

# add `/app/node_modules/.bin` to $PATH
ENV PATH /app/node_modules/.bin:$PATH
ENV GITHUB-URL $REPO-URL

RUN echo GITHUB-URL
RUN echo ${GITHUB-URL}

RUN apk update && apk upgrade && \
    apk add --no-cache bash git openssh

# Clone code
RUN git clone -b v4.0.0-alpha ${GITHUB-URL} /web/

RUN cp -rf /web/forms-flow-web/* /app/
RUN rm /app/public/formsflow.ai_icon.svg
COPY /public/formsflow.ai_icon.svg /app/public/

RUN npm install
RUN npm run build

FROM nginx:1.17 as production-stage
RUN mkdir /app
COPY --from=build-stage /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 8080:8080
CMD ["nginx", "-g", "daemon off;"]