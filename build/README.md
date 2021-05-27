#  在工业中 build一个 Dockerfile 

### 使用 --build-arg 传入 arg 在build time时， 在用 env 使用传入的值

Dockerfile: 
```
ARG orgname_biz_org='org name '
ARG orgname_biz_unit='unit name'
ARG orgname_build_by
ARG orgname_build_date
ARG orgname_build_id
ARG orgname_build_number
ARG orgname_src_author
ARG orgname_src_branch
ARG orgname_src_version

ENV INFO_orgname_BIZ_ORG=$orgname_biz_org
ENV INFO_orgname_BIZ_UNIT=$orgname_biz_unit
ENV INFO_orgname_BASEIMAGE_BIZ_ORG=$orgname_biz_org
ENV INFO_orgname_BASEIMAGE_BIZ_UNIT=$orgname_biz_unit
ENV INFO_orgname_BASEIMAGE_BUILD_BY=$orgname_build_by
ENV INFO_orgname_BASEIMAGE_BUILD_DATE=$orgname_build_date
ENV INFO_orgname_BASEIMAGE_BUILD_ID=$orgname_build_id
ENV INFO_orgname_BASEIMAGE_BUILD_NUMBER=$orgname_build_number
ENV INFO_orgname_BASEIMAGE_SRC_AUTHOR=$orgname_src_author
ENV INFO_orgname_BASEIMAGE_SRC_BRANCH=$orgname_src_branch
ENV INFO_orgname_BASEIMAGE_SRC_VERSION=$orgname_src_version

ENTRYPOINT /usr/bin/start.sh
```

Build docker command: 
```
        docker build \
        -t "${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}-${TIMESTAMP}" \
        -t "${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}" \
        . \
        --build-arg orgname_biz_org='org name' \
        --build-arg orgname_biz_unit='unit name' \
        --build-arg orgname_build_by="${BUILD_QUEUEDBY}" \
        --build-arg orgname_build_date="${TIMESTAMP}" \
        --build-arg orgname_build_id="${BUILD_BUILDID}" \
        --build-arg orgname_build_number="${BUILD_BUILDNUMBER}" \
        --build-arg orgname_src_author="${BUILD_SOURCEVERSIONAUTHOR}" \
        --build-arg orgname_src_branch="${BUILD_SOURCEBRANCHNAME}" \
        --build-arg orgname_src_version="${BUILD_SOURCEVERSION}" \
        --label "com.orgname.build.by=${BUILD_QUEUEDBY}" \
        --label "com.orgname.build.date=${TIMESTAMP}" \
        --label "com.orgname.build.id=${BUILD_BUILDID}" \
        --label "com.orgname.build.number=${BUILD_BUILDNUMBER}" \
        --label "com.orgname.src.author=${BUILD_SOURCEVERSIONAUTHOR}" \
        --label "com.orgname.src.branch=${BUILD_SOURCEBRANCHNAME}" \
        --label "com.orgname.src.version=${BUILD_SOURCEVERSION}" \
        --label "com.orgname.docker.cmd=docker run ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}-${TIMESTAMP}" ||
        {
          echo "##[error]ERROR: Failed to build docker image!"
          exit 1
        }
```
