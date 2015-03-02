FROM google/debian:wheezy

RUN apt-get update -y && apt-get -y upgrade
RUN apt-get install --no-install-recommends -y -q curl build-essential ca-certificates git vim less cmake ruby
RUN gem install redis
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

ENV REDIS_MAJOR 3.0
ENV REDIS_VERSION 3.0.0-rc4
ENV REDIS_REPLICATION_MASTER **False**
ENV REDIS_REPLICATION_SLAVE **False**

RUN mkdir /var/lib/redis
RUN mkdir -p /etc/redis
RUN curl -Lso /tmp/redis-"${REDIS_VERSION}".tar.gz https://github.com/antirez/redis/archive/"${REDIS_VERSION}".tar.gz
RUN tar zxf /tmp/redis-"${REDIS_VERSION}".tar.gz -C /usr/local/src/
RUN cd /usr/local/src/redis-"${REDIS_VERSION}" && make -j4 MALLOC=jemalloc && make install
RUN ln -s /usr/local/src/redis-"${REDIS_VERSION}"/src/redis-trib.rb /usr/local/bin/redis-trib.rb

ENV PATH /usr/local/redis/bin:$PATH

ADD ./redis.conf /etc/redis/redis.conf
ADD ./bootstrap.sh /tmp/bootstrap.sh
RUN chmod +x /tmp/bootstrap.sh
ADD ./redis-proto.sh /usr/local/bin/redis-proto.sh
RUN chmod +x /usr/local/bin/redis-proto.sh

VOLUME ["/var/lib/redis"]
EXPOSE 6379 16379
ENTRYPOINT ["/tmp/bootstrap.sh"]
CMD ["redis-server", "/etc/redis/redis.conf"]
