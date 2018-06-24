## zzcas1 ##
	zero@zzcas1 ~ $ cat /data/default/cas
	export DBPORT=30000
	export HTTP=8080
	export NFS=zzcas1
	export ZONE=cas-zzja.xiyoux.com:30002
	export CAS=cas
	
	export RPORTS=cas-zzja.xiyoux.com:30002,cas-cqff.xiyoux.com:30002,bjuk.xiyoux.com:30002

## zzcas2 ##

	zero@zzcas2 /data/default $ cat cas1 
	export HTTP=8081
	export ZONE=cas-zzja.xiyoux.com:30003
	export CAS=cas1
	export DB=cas1
	export MAIN=cas1
	export NFS=zzcas2

	export RPORTS=cas-zzja.xiyoux.com:30003,cas-cqff.xiyoux.com:30003,bjuk.xiyoux.com:30003

## zzcas3 ##
	zero@zzcas3 /data/default $ cat cas2  
	export NFS=zzcas3
	export ZONE=cas-zzja.xiyoux.com:30001
	export CAS=cas2
	export PORT=8082

	export RPORTS=cas-zzja.xiyoux.com:30001,cas-cqff.xiyoux.com:30001,bjuk.xiyoux.com:30001

## cqcas1 ##
	zero@cqcas1 ~ $ cat /data/default/cas 
	export DBPORT=30000
	export HTTP=8080
	export NFS=cqcas1
	export ZONE=cas-cqff.xiyoux.com:30002
	export CAS=cas

	export RPORTS=cas-zzja.xiyoux.com:30002,cas-cqff.xiyoux.com:30002,bjuk.xiyoux.com:30002

cqcas2
	