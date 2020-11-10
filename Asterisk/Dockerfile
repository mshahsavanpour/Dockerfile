FROM centos:7


LABEL maintainer "Mina ShahsavanPour <m.shahsavanpour@riseup.net>"

ENV VERSION 17.4.0

RUN yum -y update \ 
&& yum -y groupinstall "Development Tools" \
&& yum -y install openssl openssl-devel libedit-devel sqlite-devel psmisc gmime-devel ncurses-devel libtermcap-devel sox newt-devel libxml2-devel libtiff-devel audiofile-devel gtk2-devel uuid-devel libtool libuuid-devel kernel-devel kernel-devel-$(uname -r) git  kernel-devel crontabs cronie wget ncurses-devel mysql-connector-odbc unixODBC unixODBC-devel opus speex libsrtp-devel mariadb-server net-snmp-devel \
&& cd /usr/src \
&& wget -O libsrtp-2.tar.gz http://github.com/cisco/libsrtp/archive/v2.tar.gz \
&&  tar -xf libsrtp-2.tar.gz \
&& cd libsrtp-2 \
&& ./configure --enable-openssl \
&& make shared_library install \
&& echo "/usr/local/lib" > /etc/ld.so.conf.d/usr_local.conf \
&& ldconfig \
&& cd /usr/src \
&& git clone https://github.com/akheron/jansson.git \
&& cd jansson \
&& autoreconf  -i \
&& ./configure --prefix=/usr/ \
&& make \
&& make install \
&& cd /usr/src/ \
&& git clone https://github.com/pjsip/pjproject.git \
&& cd pjproject \
&& ./configure CFLAGS="-DNDEBUG -DPJ_HAS_IPV6=1" --prefix=/usr --libdir=/usr/lib64 --enable-shared  --disable-video --disable-sound --disable-opencore-amr \
&& make dep \
&& make \
&& make install \
&& ldconfig \
&& cd /usr/src \
&& wget http://downloads.asterisk.org/pub/telephony/asterisk/releases/asterisk-${VERSION}.tar.gz \
&& tar -xzvf asterisk-${VERSION}.tar.gz \
&& cd /usr/src/asterisk-${VERSION}* \
    && ./configure CFLAGS='-g -O2 -mtune=native' --libdir=/usr/lib64  \
    && make menuselect.makeopts \
    && menuselect/menuselect \
    --enable res_config_mysql \
    --enable codec_opus \
    --enable DONT_OPTIMIZE \
    --enable res_chan_stats \
    --enable res_endpoint_stats \  
    --enable res_srtp \
    --enable res_snmp \
    --enable func_curl \
    menuselect.makeopts \
    && make -j2 \
    && make install \
    && make samples \
    && yum clean all \
    && groupadd asterisk \
    && useradd -r -d /var/lib/asterisk -g asterisk asterisk \
    && usermod -aG audio,dialout asterisk \
    && chown -R asterisk.asterisk /etc/asterisk \
    && chown -R asterisk.asterisk /var/{lib,log,spool}/asterisk \
    && chown -R asterisk.asterisk /usr/lib64/asterisk

CMD /usr/sbin/safe_asterisk


