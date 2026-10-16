# lite + HTTP/2, aloli
PKGNAMESUFFIX=	-lite-aloli
COMMENT=	Robust and small WWW server (lite + HTTP/2, aloli)

MASTERDIR=	${PORTSDIR}/www/nginx

# Paquet MAISON : nom distinct de tout paquet officiel (jamais de chevauchement).
CONFLICTS_INSTALL=	nginx nginx-devel nginx-full nginx-lite nginx-naxsi \
			nginx-lite-quimeo nginx-lite-passenger-*

# Options de nginx-lite (HTTP, HTTP_SSL, IPV6, FILE_AIO, THREADS) + HTTPV2 ;
# sans cache proxy ni realip (non utilisés).
OPTIONS_DEFAULT=	FILE_AIO HTTP HTTP_SSL HTTPV2 IPV6 THREADS

.include "${MASTERDIR}/Makefile"
