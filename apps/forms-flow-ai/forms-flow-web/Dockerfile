# base image
FROM node:12.2.0-alpine as build-stage

# add `/app/node_modules/.bin` to $PATH
ENV PATH /app/node_modules/.bin:$PATH
ENV REPOURL=${REPOURL}
ENV REPOBRANCH=${REPOBRANCH}

RUN echo ${REPOURL}
RUN echo ${REPOBRANCH}

# set working directory
WORKDIR /app

RUN apk update && apk upgrade && \
    apk add --no-cache bash git openssh

# Clone code
RUN git clone -b ${REPOBRANCH} ${REPOURL} /web/

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