#============================================================================================
# IMPORTANTE

**IMPORTANTE**
Este es el python usado por galaxy --> [/home/horus/software/galaxy/.venv/bin/python2.7]

## Setear un python mas completo
```bash
rm
/home/horus/software/galaxy/.venv/bin/python
ln -s /home/horus/.anaconda2/bin/python python
```


#============================================================================================
# Tutoriales y documentos

## Como setear galaxy
**https://wiki.galaxyproject.org/Events/GCC2014/TrainingDay/AdminWalkthrough**

## Otros documentos y lecturas
**configuración**
Production Server		--> https://wiki.galaxyproject.org/Admin/Config/Performance/ProductionServer
Data Tables			--> https://wiki.galaxyproject.org/Admin/Tools/Data%20Tables

**galaxy with SGE**		--> https://wiki.galaxyproject.org/Admin/Config/Performance/Cluster

**add tool**			--> https://wiki.galaxyproject.org/Admin/Tools/AddToolTutorial

**data managers**
Data managers			--> https://wiki.galaxyproject.org/Events/GCC2014/TrainingDay/DataManagers
Data managers de otra pagina    --> http://bioinformatics.ucdavis.edu/research-computing/documentation/using-datamanagers-to-create-your-own-built-in-genomes/

**docker + galaxy**
Preguntas importanes    	--> https://biostar.usegalaxy.org/p/11376/
Ultima version          	--> https://biostar.usegalaxy.org/p/16257/
otras cosillas          	--> https://hub.docker.com/r/bgruening/galaxy-stable/
repo                    	--> https://github.com/bgruening/docker-galaxy-stable
docker galaxy + jupyter         --> https://github.com/bgruening/galaxy-ipython
docker images			--> https://biostar.usegalaxy.org/p/7375/
install galaxy + docker 	--> http://www.learnfaceit.org/for-developers/installing-a-galaxy-es-virtual-machine
exome + docker			--> https://github.com/bgruening/docker-recipes/blob/master/galaxy-exom-seq/README.md

**otros**
galaxy + jupyter 		--> https://hub.docker.com/r/bgruening/galaxy-stable/
galaxy wrapper tools		--> https://github.com/bgruening/galaxytools
Sample Tracking			--> https://wiki.galaxyproject.org/Admin/DataLibraries/LibrarySampleTracking


#============================================================================================
# Instalacion de Galaxy en la notebook del trabajo con propósito de testear

## Requerimientos
**postgresql**
`sudo apt-get install libpq-dev`

**IGB: Integrated Genome Browser**
Para visualizar alineamientos --> [http://bioviz.org/igb/releases/current/IGB_unix_current.sh]

**r packages**
```bash
sudo R
install.packages("gtools");
install.packages("gdata");
```

## Instalar galaxy
**clonar e instalar**
```bash
cd ~/software
git clone https://github.com/galaxyproject/galaxy/
cd galaxy
git checkout -b master origin/master
sh run.sh
```

**configurar**
```bash
cp config/galaxy.ini.sample config/galaxy.ini
nano config/galaxy.ini
```

* Host (visible on all machines in the network)
To access Galaxy over the network, simply modify the **config/galaxy.ini** file and change the host setting to
`host = 0.0.0.0`

* The port on which to listen.
`port = 8081`

### Configurar Galaxy para que corra con postgresql ###

[detener el proceso de galaxy manualmente]

**instalar postgresl**
sudo apt-get install postgresql
sudo -u postgres psql postgres
\password postgres
[enter "postgres" como la password]
ctrl-d
psql -h localhost -U postgres
CREATE DATABASE galaxy_prod;
GRANT ALL ON DATABASE galaxy_prod TO postgres;
ctrl-d
nano ~/galaxy-dist/universe_wsgi.ini
[aagregar la siguiente linea a la sección [app:main] del archivo de configuración]
database_connection = postgres://postgres:postgres@localhost:5432/galaxy_prod
[guardar & salir]
cd ~/software/galaxy
python scripts/scramble.py -e psycopg2
sh run.sh
[las tablas migrarán a postgresql]


### Instalar iPython en galaxy
```bash
cd ~/software/galaxy
git clone https://github.com/bgruening/galaxy-ipython.git config/plugins/visualizations/ipython
```

#============================================================================================
# Testeos
1. Crear usuario
email: cristian.rohr@indear.com
user: crohr

2. Darle derechos de administrador al usuario
In order to control your new Galaxy through the UI (installing tools, managing users, creating groups etc.) you have to become an administrator. First register as a new user and then give the user admin privileges like this: You add the Galaxy login ( email ) to the Galaxy configuration file (**config/galaxy.ini**). If the file does not exist you can copy it from the provided sample (config/galaxy.ini.sample). Note that you have to restart Galaxy after modifying the configuration for changes to take effect.
`admin_users = cristian.rohr@indear.com`

3. Instalar herramientas de la tool shed

[Crear una carpeta donse se instalarán las dependencias]
```bash
cd ~/software/galaxy
mkdir dependencias
```

[Cambiar archivo de configuración]
# Path to the directory in which tool dependencies are placed.  This is used by
# the tool shed to install dependencies and can also be used by administrators
# to manually install or link to dependencies.  For details, see:
#   https://wiki.galaxyproject.org/Admin/Config/ToolDependencies
# If this option is not set to a valid path, installing tools with dependencies
# from the Tool Shed will fail.
`tool_dependency_dir = dependencias`

Clickear en Admin -> Search Tool Shed

* NGS: Mapping 
	- bwa [ 11 (2016-01-19) ]

* NGS: QC & Manipulation
	- fastq_groomer [ 1 (2015-11-11) ]
	- fastqc [ 7 (2016-04-11) ]
	- trimmomatic [ 3 (2015-09-23) ]

* NGS: Picard
	- picard [ 11 (2015-11-11) ]

* NGS: Samtools 
	- suite_samtools_1_2

* NGS: Variant Calling
	- GATK [ 0 (2015-09-24) ]

* NCBI-BLAST
	- ncbi_blast_plus [ 19 (2015-11-19) ]

4. Instalar Data Managers
* Algunos importantes -->
	**data_manager_fetch_genome_all_fasta** 
	**data_manager_bwa_index_builder**
	**data_manager_gatk_picard_index_builder**

* Pasos
Instalar data managers para bajar el genoma de referencia y construir los indices para BWA:
- Click Admin from the masthead
- Click Search tool sheds from the left panel
- Click the popup icon for Galaxy main tool shed
- Search for Tool ids that contain the name __manager__
- Check **data_manager_fetch_genome_all_fasta** and **data_manager_bwa_index_builder**, then click Install to Galaxy (at the bottom of the page)
- Click Install

5. Obtener hg19
- Click en **Admin**
- Click en **Local data** en la seccion [Data]
- Click en **Reference Genome** en la seccion [Run Data Manager Tools]
- Seleccionar __hg19__ en la seccion [DBKEY to assign to data]
- Click en **Execute**

6. Crear índice para bwa
- Click en Admin
- Click en Local data en la seccion Data
- Click en **BWA index** en la seccion [Run Data Manager Tools]
- Choose Human Feb. 2009 (GRCh37/hg19)(hg19) en [Source FASTA Sequence]
- hg19 [Name of sequence]
- hg19 [ID for sequence]
- Edit the file --> /home/horus/software/galaxy/tool-data/toolshed.g2.bx.psu.edu/repos/devteam/bwa/546ada4a9f43/bwa_mem_index.loc
  add --> [hg19	hg19	hg19	/home/horus/software/galaxy/tool-data/hg19/bwa_index/hg19/hg19.fa]
- Stop & Start galaxy

7. Crear diccionario de picard
```bash
cd ~/software/galaxy/tool-data/hg19
mkdir picard
cd picard
ln -s ../seq/hg19.fa .
java -jar /home/horus/software/galaxy/dependencias/picard/1.136/devteam/package_picard_1_136/3e9c24e5325b/picard.jar CreateSequenceDictionary R=hg19.fa O=hg19.dict
samtools faidx hg19.fa
```

8. Configurar GATK
cd /home/horus/software/galaxy/tool-data/toolshed.g2.bx.psu.edu/repos/avowinkel/gatk/b80ff7f43ad1
gedit picard_index.loc
add --> [hg19	hg19	hg19	/home/horus/software/galaxy/tool-data/hg19/picard/hg19.fa]

Es necesario instalar GATK de forma local e indicarle al wrapper donde se ubica
edit --> /home/horus/software/galaxy/dependencias/environment_settings/GATK_PATH/avowinkel/gatk/b80ff7f43ad1/env.sh
add --> GATK folder

9. Probar el siguiente pipeline ubicado en **argentum** --> [/data/projects/cartamo_spc/xx_dossier_regulatorio_L15]
- Crear una carpeta para almacenar mis herramientas. Tutorial --> [https://wiki.galaxyproject.org/Admin/Tools/AddToolTutorial]
```bash
cd ~/software/galaxy/tools
mkdir myTools
cd myTools
scp srevale@argentum:/data/projects/cartamo_spc/xx_dossier_regulatorio_L15/xx_scripts/\*.py .
```

At this point toolExample.pl resides within the tools/myTools/ directory, but Galaxy still does not know how to run this tool. To let Galaxy know the execution details of our new tool, we need to write a tool configuration file, which, in this case, will be called toolExample.xml. For this particular example open a text editor of your choice and create the toolExample.xml file within the tools/myTools directory. This file looks like this:

<tool id="fa_gc_content_1" name="Compute GC content" version="0.1.0">
  <description>for each sequence in a file</description>
  <command interpreter="perl">toolExample.pl $input $output</command>
  <inputs>
    <param format="fasta" name="input" type="data" label="Source file"/>
  </inputs>
  <outputs>
    <data format="tabular" name="output" />
  </outputs>

  <tests>
    <test>
      <param name="input" value="fa_gc_content_input.fa"/>
      <output name="out_file1" file="fa_gc_content_output.txt"/>
    </test>
  </tests>

  <help>
This tool computes GC content from a FASTA file.
  </help>

</tool>

```bash
cd ~/software/galaxy/conf
cp tool_conf.xml.sample tool_conf.xml
```

Edit the tool_conf.xml on the config directory, add

  <section id="MyTools" name="mTools">
	<tool file="myTools/find_orfs.xml"/>
  </section>


# Finalmente para que esto funcione hice lo siguiente:
Modifique el script de python, para que solo genere un archivo de salida.
Tambien lo modifique para que no muestre warnings.

El archivo xml de la herramienta quedo asi
<tool id="find_orfs_1" name="Find Orfs" version="0.1.0">
  <description>for each sequence in a file</description>
<!--    <command interpreter="python">gcContent.pl -f ${SEQUENCE_FILE} -x ${PREFIX} -g -p -k -l 60 -m ${MIN_PEPTIDE_LENGTH}</command>-->
	<command interpreter="python">find_orfs.py -f ${SEQUENCE_FILE} -x ${output} -k -l 60 -m ${MIN_PEPTIDE_LENGTH}</command>
<!--    <command interpreter="python">find_orfs.py -f ${SEQUENCE_FILE} -x salida_probando -g -p -k -l 60 -m 5</command>-->
  <inputs>
    <param format="fasta" name="SEQUENCE_FILE" type="data" label="Source file"/>
    <param name="MIN_PEPTIDE_LENGTH" size="20" type="text" value="10" label="MIN PEPTIDE LENGTH"/>
  </inputs>

  <outputs>
    <data format="tabular" name="output" />
  </outputs>
<!--
  <tests>
    <test>
      <param name="input" value="fa_gc_content_input.fa"/>
      <output name="out_file1" file="fa_gc_content_output.txt"/>
    </test>
  </tests>
-->
  <help>
This tool find orfs in a FASTA file.
  </help>

</tool>

10. Instale el wrapper para ncbi-blast ncbi_blast_plus [ 19 (2015-11-19) ]
- Formateo la base de datos con makeblastdb 
`makeblastdb -input uniprot_sprot.fasta -dbtype prot`

- Agregue la base de datos de uniprot, al archivo de localizaciones de bases de datos de proteinas para blast
[/home/horus/software/galaxy/tool-data/toolshed.g2.bx.psu.edu/repos/devteam/ncbi_blast_plus/7f3c448e119b/blastdb_p.loc]




#============================================================================================
# Descripcion de archivos y recursos
+ Reference Genome --> *.fasta [Estos archivos fasta necesitan indices y nombres DBKEY's]
+ BWA Index Files on disk -->  *.amb, *.bwt, *.rsa, *.ann, etc.
+ File listing paths, genome build, descriptions, etc. of available BWA indexes --> bwa_index.loc
+ Tool Data Table --> tool_data_table_conf.xml [Los data managers -> llenan los tool data tables ~ *.loc files]
+ Galaxy BWA Tool -> bwa_wrapper.xml [No necesitan comunicarse con los .loc, se comunica con el tool_data_table]

* *. loc files: 
	- Used as a way to provide additional configuration details to a tool, without having to manually edit the actual tool XML file.
	- Colloquially referred to as "loc" or "location" files.
	- Often used to store the path (location on disk) of reference data and indexes, along with appropriate metadata (display names, dbkeys/genome 		builds)
	- Need not end in the suffix of ".loc", although they commonly do by convention.
	- **Tab** delimited flat files, where each row is an entry in the table.
	- Should **not be accessed directly** in a tool. The Tool Data Tables abstraction layer should be used.
