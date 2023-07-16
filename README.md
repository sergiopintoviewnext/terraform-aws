## DESCRIPCIÓN 


Este playbook contiene dos roles para la instalacion de terraform y la instalacion y configuracion de aws-cli en las distribuciones de Linux:

- Debian/Ubuntu
- Redhat
- Suse

## VARIABLES


En rol terraform:

Debido a que suse no tiene repositorio propio para terraform, se instala descargando el binario y copiandolo en /usr/bin y se requieren dos variables:

- path: contiene la ruta de la carpeta temporal que contendrá la descarga del binario. Se elimina al final del rol.
- version_suse: contiene la version de terraform que se descargará para la distribución Suse

Para las distribuciones de Debian y Rhel no se requieren variables ni indicar version ya que se añade el repositorio y descargará la ultima version.

En rol aws-cli:

Para la configuración de aws-cli se requieren las siguientes variables:

- access_key: access_key de aws del usuario iam
- secret_key: la secret_key de aws del usuario iam
- region: region de aws
- format: formato


## PLAYBOOK


			- host: all
			  become: true
			  roles:
				  - role: terraform
				  - role: aws-cli


## TEST

Se ha realizado el testeo de este rol mediante:

- Molecule:

		molecule 4.0.4 using python 3.9
		ansible:2.14.3
		docker:2.1.0 from molecule_docker requiring collections: community.docker>=3.0.2 ansible.posix>=1.4.0

	utilizando las imagenes de docker:
		- registry.access.redhat.com/ubi8/ubi-init:8.8-8 
		- registry.access.redhat.com/ubi9/ubi-init:9.2-5
		- docker.io/debian:stable-20230703
		- docker.io/ubuntu:jammy-20230624
		
- AWS:

		- Red Hat Enterprise Linux 9
		- Debian 11
		- Ubuntu Server 22.04 
		- SUSE Linux Enterprise Server 15