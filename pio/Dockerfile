#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

FROM openjdk:8

ENV PIO_TAG=v0.12.1
ENV SCALA_VERSION=2.11.12
ENV SPARK_VERSION=2.2.2
ENV HADOOP_VERSION=2.7.7
ENV ELASTICSEARCH_VERSION=5.5.3

WORKDIR /opt/src
RUN git clone https://github.com/apache/predictionio.git
WORKDIR /opt/src/predictionio
RUN git checkout refs/tags/$PIO_TAG
RUN bash ./make-distribution.sh \
      -Dscala.version=$SCALA_VERSION \
      -Dspark.version=$SPARK_VERSION \
      -Dhadoop.version=$HADOOP_VERSION \
      -Delasticsearch.version=$ELASTICSEARCH_VERSION

