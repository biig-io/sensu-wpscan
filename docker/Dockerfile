
FROM ubuntu:16.04

MAINTAINER Manuel BARRAUD <manuel.barraud@biig.fr>

ENV DEBIAN_FRONTEND noninteractive

ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8

RUN apt-get update && \
    apt-get -y --no-install-recommends install curl wget git ca-certificates locales&& \
	locale-gen en_US.UTF-8 && \
    #install sensu key
    wget -q https://sensu.global.ssl.fastly.net/apt/pubkey.gpg -O- | apt-key add - && \
    echo "deb http://sensu.global.ssl.fastly.net/apt xenial main" | tee /etc/apt/sources.list.d/sensu.list && \
    apt-get update && \
    curl -sSL https://rvm.io/mpapis.asc | gpg --import - && \
    curl -sSL https://get.rvm.io | bash -s stable && \
    bash -c "source /usr/local/rvm/scripts/rvm" && \
    echo "source /usr/local/rvm/scripts/rvm" >> ~/.bashrc && \
    bash -c "source /usr/local/rvm/scripts/rvm ;rvm install 2.5.0 ;rvm use 2.5.0 --default" && \
    echo "gem: --no-ri --no-rdoc" > ~/.gemrc && \
    cd /opt/ && git clone https://github.com/wpscanteam/wpscan.git && \
    cd /opt/wpscan/ && \
    bash -c "source /usr/local/rvm/scripts/rvm ; gem install bundler ; bundle install --without test" && \
    ln -s /usr/lib/x86_64-linux-gnu/libcurl-gnutls.so.4 /usr/lib/x86_64-linux-gnu/libcurl.so.4 && \
	apt-get install sensu && \
    sensu-install -p wordpress && \
	#Need to run wpscan (ruby 2.5) launch by sensu (ruby 2.4)
	ln -s /usr/local/rvm/wrappers/ruby-2.5.0\@wpscan/ruby /usr/local/bin/ruby-2.5.0-wpscan && \
	apt-get -y remove --purge libgcc-5-dev libc6-dev dpkg-dev libgmp-dev libffi-dev perl git make patch m4 && \
	apt-get -y autoremove && \
	cd /opt/wpscan &&  bash -c "source /usr/local/rvm/scripts/rvm ; ./wpscan.rb --update" && \
	rm /opt/wpscan/data.zip && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN sed -i 's=env ruby=env /usr/local/bin/ruby-2.5.0-wpscan=g' /opt/wpscan/wpscan.rb
WORKDIR /opt/wpscan
CMD ["/opt/sensu/bin/sensu-service","start","client"]


