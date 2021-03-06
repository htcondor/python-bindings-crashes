# Copyright 2019 HTCondor Team, Computer Sciences Department,
# University of Wisconsin-Madison, WI.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM centos:centos7

# switch to root to do root-level config
USER root

# build config
ARG HTCONDOR_VERSION=8.9

RUN yum install -y epel-release wget \
 && yum clean all

# install HTCondor version specified in config
RUN wget https://research.cs.wisc.edu/htcondor/yum/RPM-GPG-KEY-HTCondor \
 && rpm --import RPM-GPG-KEY-HTCondor \
 && yum-config-manager --add-repo https://research.cs.wisc.edu/htcondor/yum/repo.d/htcondor-development-rhel7.repo \
 && yum install -y condor-all python3-condor \
 && yum clean all

RUN yum install -y python3-pip python-dev python3-dev sudo git vim nano less top htop where which man tmux strace gdb \
 && debuginfo-install -y python boost169-python boost169-python2 boost169-python3 python2-condor python3-condor zlib \
 && yum clean all

ENV USER=tester
RUN adduser $USER && usermod -aG wheel $USER && echo "%wheel  ALL=(ALL)   NOPASSWD:ALL" >> /etc/sudoers

COPY --chown=tester:tester docker/entrypoint.sh /.entrypoint.sh
RUN chmod +x /.entrypoint.sh
COPY --chown=tester:tester docker/condor_config.local /etc/condor/condor_config.local
ENTRYPOINT ["/.entrypoint.sh"]

# switch to submit user, don't need root anymore
USER ${USER}

WORKDIR /home/tester/crashes
CMD ["bash", "-l"]
