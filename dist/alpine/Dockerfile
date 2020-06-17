FROM python:3.8-alpine3.12 as k8s_build
ARG KOPS_VERSION
ARG HELM_VERSION
RUN \
    apk add curl jq && \
    # AWS CLI
    pip install awscli --upgrade --user && \
    # KOPS
    curl -L $(echo "https://github.com/kubernetes/kops/releases/download/${KOPS_VERSION}\
		/kops-linux-amd64" -o /usr/local/bin/kops | tr -d '\n' | tr -d '\t') && \
    chmod +x /usr/local/bin/kops && \
    # KUBECTL
    mkdir ${HOME}/.kube && \
    curl -L $(echo "https://storage.googleapis.com/kubernetes-release/release/\
		$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)\
		/bin/linux/amd64/kubectl" -o /usr/local/bin/kubectl | tr -d '\n' | tr -d '\t') && \
    chmod +x /usr/local/bin/kubectl && \
    # HELM
    curl -L https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz -o /tmp/helm.tar.gz && \
    tar -zxvf /tmp/helm.tar.gz --strip-components=1 -C /usr/local/bin/ linux-amd64/helm && \
    rm /tmp/helm.tar.gz && \
    chmod +x /usr/local/bin/helm && \
    helm init --history-max 200 --client-only
ENV PATH "/root/.local/bin:$PATH"
RUN mkdir /data && mkdir src

# #Squashed Final Image
FROM scratch
COPY --from=k8s_build / /
LABEL maintainerName="Raphael Guzman" \
      maintainerEmail="raphael@vathes.com" \
      maintainerCompany="DataJoint"
ENV PATH "/root/.local/bin:$PATH"
WORKDIR /src
VOLUME /src
VOLUME /data
CMD ["sh"]