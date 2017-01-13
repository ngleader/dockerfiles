FROM centos:centos7
MAINTAINER ngleader <ngleader@gmail.com>

# epel repo
RUN yum -y install epel-release && \
	yum -y update && \
	yum clean all

# set locale ko_KR
RUN localedef -f UTF-8 -i ko_KR ko_KR.UTF-8
ENV LANG ko_KR.UTF-8
ENV LANGUAGE ko_KR.UTF-8
ENV LC_ALL ko_KR.UTF-8