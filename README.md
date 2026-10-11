# Bài 3: Tối ưu hóa dung lượng Image & Layer Caching với Multi-stage Build

```bash
dev_docker@ip-172-31-42-200:~/PTIT_CNTT1_IT209_TestDeploy-VPS$ docker build -t hw-backend:single -f Dockerfile.single .
[+] Building 110.3s (9/9) FINISHED                                                                                                                                                                  docker:default
 => [internal] load build definition from Dockerfile.single                                                                                                                                                   0.0s
 => => transferring dockerfile: 437B                                                                                                                                                                          0.0s
 => [internal] load metadata for docker.io/library/eclipse-temurin:17-jdk                                                                                                                                     1.1s
 => [internal] load .dockerignore                                                                                                                                                                             0.0s
 => => transferring context: 2B                                                                                                                                                                               0.0s
 => [1/4] FROM docker.io/library/eclipse-temurin:17-jdk@sha256:5d6042fb8cdc14d614e4e421f52e3211fc5aeadddce44cbf9de8ed37c791f824                                                                               0.0s
 => => resolve docker.io/library/eclipse-temurin:17-jdk@sha256:5d6042fb8cdc14d614e4e421f52e3211fc5aeadddce44cbf9de8ed37c791f824                                                                               0.0s
 => [internal] load build context                                                                                                                                                                             0.0s
 => => transferring context: 28.72kB                                                                                                                                                                          0.0s
 => CACHED [2/4] WORKDIR /app                                                                                                                                                                                 0.0s
 => [3/4] COPY . .                                                                                                                                                                                            0.3s
 => [4/4] RUN chmod +x ./gradlew && ./gradlew clean bootJar -x test --no-daemon                                                                                                                              76.0s
 => exporting to image                                                                                                                                                                                       31.8s
 => => exporting layers                                                                                                                                                                                      21.0s
 => => exporting manifest sha256:6f5254760572a8ed74d2169fa9719a34392e932a8a46945b989d7bd4943adc06                                                                                                             0.0s
 => => exporting config sha256:c1dea65b577af10ef42a981d69268dd4da76203bc6ac6f4343a801381526f992                                                                                                               0.0s
 => => exporting attestation manifest sha256:773161f9bc50f3c032a67bea3a73a3fcad950a1829a8f6492bc42aebec5dcbf1                                                                                                 0.0s
 => => exporting manifest list sha256:8382b72d466b7e851815272bfaeeb2eac031343a565b2e2cb69f56a082cbfbd8                                                                                                        0.0s
 => => naming to docker.io/library/hw-backend:single                                                                                                                                                          0.0s
 => => unpacking to docker.io/library/hw-backend:single                                                                                                                                                      10.4s
dev_docker@ip-172-31-42-200:~/PTIT_CNTT1_IT209_TestDeploy-VPS$ docker images | grep hw-backend
hw-backend:multistage                            cf2bd87e3fcd        368MB          119MB
hw-backend:single                                8382b72d466b       1.37GB          509MB
dev_docker@ip-172-31-42-200:~/PTIT_CNTT1_IT209_TestDeploy-VPS$ docker build -t hw-backend:multistage .
[+] Building 8.7s (13/13) FINISHED                                                                                                                                                                  docker:default
 => [internal] load build definition from Dockerfile                                                                                                                                                          0.0s
 => => transferring dockerfile: 417B                                                                                                                                                                          0.0s
 => [internal] load metadata for docker.io/library/eclipse-temurin:17-jre-alpine                                                                                                                              1.3s
 => [internal] load metadata for docker.io/library/eclipse-temurin:17-jdk                                                                                                                                     1.3s
 => [internal] load .dockerignore                                                                                                                                                                             0.0s
 => => transferring context: 2B                                                                                                                                                                               0.0s
 => [builder 1/4] FROM docker.io/library/eclipse-temurin:17-jdk@sha256:5d6042fb8cdc14d614e4e421f52e3211fc5aeadddce44cbf9de8ed37c791f824                                                                       0.0s
 => => resolve docker.io/library/eclipse-temurin:17-jdk@sha256:5d6042fb8cdc14d614e4e421f52e3211fc5aeadddce44cbf9de8ed37c791f824                                                                               0.0s
 => [internal] load build context                                                                                                                                                                             0.1s
 => => transferring context: 11.43kB                                                                                                                                                                          0.0s
 => [stage-1 1/3] FROM docker.io/library/eclipse-temurin:17-jre-alpine@sha256:3c472129dc75a8d1d7a3f2df5b2093a8077e4493deff046754d4764d0371de63                                                                0.0s
 => => resolve docker.io/library/eclipse-temurin:17-jre-alpine@sha256:3c472129dc75a8d1d7a3f2df5b2093a8077e4493deff046754d4764d0371de63                                                                        0.0s
 => CACHED [builder 2/4] WORKDIR /app                                                                                                                                                                         0.0s
 => CACHED [builder 3/4] COPY . .                                                                                                                                                                             0.0s
 => CACHED [builder 4/4] RUN chmod +x ./gradlew && ./gradlew clean bootJar -x test --no-daemon                                                                                                                0.0s
 => CACHED [stage-1 2/3] WORKDIR /app                                                                                                                                                                         0.0s
 => [stage-1 3/3] COPY --from=builder /app/build/libs/*.jar app.jar                                                                                                                                           0.2s
 => exporting to image                                                                                                                                                                                        3.2s
 => => exporting layers                                                                                                                                                                                       2.5s
 => => exporting manifest sha256:5354c2a75c8af53f5437e2f6fb8c7e265ef81ba853d1040632ff67fc240e8904                                                                                                             0.0s
 => => exporting config sha256:cfedf664f56551a0a78ee93ac93426b68eefb3fd1067114635a9d9ee2641d16f                                                                                                               0.0s
 => => exporting attestation manifest sha256:313144a6cd1ed202872725d3fef4964f33a154540ed30ae41ccfe7e6872dddc1                                                                                                 0.0s
 => => exporting manifest list sha256:1c1366307457b356ab8cf8cf416da6e92cbd557d27cd509d9c87d5f55ce27b04                                                                                                        0.0s
 => => naming to docker.io/library/hw-backend:multistage                                                                                                                                                      0.0s
 => => unpacking to docker.io/library/hw-backend:multistage                                                                                                                                                   0.6s
dev_docker@ip-172-31-42-200:~/PTIT_CNTT1_IT209_TestDeploy-VPS$ docker images
                                                                                                                                                                                               i Info →   U  In Use
IMAGE                                            ID             DISK USAGE   CONTENT SIZE   EXTRA
alpine:latest                                    294b683cb724         13MB         3.94MB
hello-world:latest                               5e2309035332       25.9kB         9.49kB    U
hw-backend:multistage                            1c1366307457        368MB          119MB
hw-backend:single                                8382b72d466b       1.37GB          509MB
my-html-app:v1                                   24f765718332       93.6MB         26.3MB    U
mysql:8.4                                        6ea90827b110       1.12GB          255MB    U
nginx:alpine                                     df221db836e1       94.4MB         27.2MB
ptit_cntt1_it209_testdeploy-vps-backend:latest   4a451f18a067        368MB          119MB    U
```
