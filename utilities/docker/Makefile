export OVS_BRANCH=branch-2.15
export OVS_VERSION=2.15
export KERNEL_VERSION=5.8.0-53-generic
export DISTRO=debian
export GITHUB_SRC=https://github.com/openvswitch/ovs.git
export DOCKER_REPO=openvswitch/ovs

# Example:
#   make build
#   make push

REPO = ${DOCKER_REPO}
tag = ${OVS_VERSION}_${DISTRO}_${KERNEL_VERSION}

build: ; rm -f /home/nik/ovs/utilities/docker/ovs.tar.gz &&tar cvfz /tmp/ovs.tar.gz -C /home/nik/ovs . && \
cp /tmp/ovs.tar.gz /home/nik/ovs/utilities/docker/ && \
docker build -t ${REPO}:${tag} --build-arg DISTRO=${DISTRO} \
--build-arg OVS_BRANCH=${OVS_BRANCH} \
--build-arg KERNEL_VERSION=${KERNEL_VERSION} \
--build-arg GITHUB_SRC=${GITHUB_SRC} -f ${DISTRO}/Dockerfile .

.PHONY: build

push: ;docker push ${REPO}:${tag}
