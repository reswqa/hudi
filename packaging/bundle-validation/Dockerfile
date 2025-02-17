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

ARG IMAGE_TAG=spark313hive313
FROM apachehudi/hudi-ci-bundle-validation-base:$IMAGE_TAG

# configure the stack
ADD . .
ENV HUDI_CONF_DIR=$WORKDIR/conf
RUN if [[ $HIVE_HOME =~ 'hive-2' ]]; \
    then mv conf/hive-site.hive2.xml conf/hive-site.xml; fi
RUN cp conf/hive-site.xml $HIVE_HOME/conf/
RUN cp conf/hive-site.xml $SPARK_HOME/conf/
RUN cp $DERBY_HOME/lib/derbyclient.jar $SPARK_HOME/jars/
RUN cp conf/spark-defaults.conf $SPARK_HOME/conf/
RUN if [[ $SPARK_HOME =~ 'spark-3.2' ]] || [[ $SPARK_HOME =~ 'spark-3.3' ]]; \
    then printf "\nspark.sql.catalog.spark_catalog org.apache.spark.sql.hudi.catalog.HoodieCatalog\n" >> $SPARK_HOME/conf/spark-defaults.conf; fi
RUN printf "\ntaskmanager.numberOfTaskSlots: 2\n" >> $FLINK_HOME/conf/flink-conf.yaml
RUN printf "\nlocalhost\n" >> $FLINK_HOME/conf/workers
