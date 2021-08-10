FROM rh-registry.gazprom-neft.local:5000/ubi8/nodejs-12 as build

LABEL vendor="GROTEM" \
      author="Gennadii Sychev" \
      author-email="gusychev@grotem.com"

WORKDIR /opt/app-root/src
COPY . .

ENV TZ="Europe/Moscow" \
    API: http://gateway-test2.gpnsm.gsychev.ru \
    SSO: http://websso-test2.gpnsm.gsychev.ru \
    PROD="true"

RUN npm install --registry=http://nexus.gazprom-neft.local:8080/repository/npm-nodejs-external-dso/
RUN npm run "build gpn-oil-lib"
RUN npm run "build webadministration"

FROM rh-registry.gazprom-neft.local:5000/rhel8/nginx-116
COPY --from=build /opt/app-root/src/dist/websso /usr/share/nginx/html
COPY nginx.conf /etc/nginx/
CMD ["nginx", "-g", "daemon off;"]
