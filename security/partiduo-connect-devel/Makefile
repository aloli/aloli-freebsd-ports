# Version de développement : branche development de partiduo-admin, mêmes
# sources que finance/partiduo-admin-devel. Un seul connect par serveur :
# elle s'installe aux mêmes emplacements que partiduo-connect et le
# remplace.
PKGNAMESUFFIX=	-devel

COMMENT=	Partiduo identity provider (development version)

CONFLICTS_INSTALL=	partiduo-connect

PARTIDUO_ADMIN_SOURCES=	${.CURDIR}/../../finance/partiduo-admin-devel

MASTERDIR=	${.CURDIR}/../partiduo-connect

.include "${MASTERDIR}/Makefile"
