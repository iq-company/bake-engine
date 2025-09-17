ARG IMAGE_BAKER_SRC=src:latest

FROM ${IMAGE_BAKER_SRC} AS src

# Build the server on run
RUN ./gradlew build -x test
WORKDIR /server/build/libs
RUN ls -ltr

