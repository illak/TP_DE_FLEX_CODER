# TP1 - Coderhouse - DE-FLEX
## Autor: Illak Zapata

Se recomienda acceder a la versión de colab ya que explica el paso a paso del proyecto:

🔗 [ENLACE A COLAB](https://colab.research.google.com/drive/1pVPXV6G2QoeSIrqzPC1qktQLt9797npE?usp=sharing)

mientras que el archivo *TP1_local.ipynb* contiene el código necesario y adaptado para correr el proyecto de manera local.

Notar que para la versión local se requieren las siguientes variables de entorno (en un archivo `.env`):

```
NEWSAPI_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXX
REDSHIFT_HOST=XXXXXXXXXXXXXXXXXXXXXXXXXXXXX
REDSHIFT_POST=XXXX
REDSHIFT_USER=XXXXXXXXXXXXXXXXXXXXXXXXXXXXX
REDSHIFT_PASS=XXXXXXXXXXXXXXXXXXXXXXXXXXXXX
REDSHIFT_DATABASE=XXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

La tabla creada en redshift se llama `df_news_api_dw` y se usó el siguiente script para su construcción:

```sql
delete from i_zapata1989_coderhouse.df_news_api_dw;


CREATE TABLE i_zapata1989_coderhouse.df_news_api_dw (
	idrecord bigint identity(1,1),
	author varchar(256),
	content varchar(1000),
	description varchar(1000),
	publishedat timestamp,
	title varchar(1000),
	url varchar(1000),
	urltoimage varchar(1000),
	source_name varchar(256),
	source_id varchar(256),
	primary key(idRecord)
) distkey(source_name) sortkey(publishedat);
```

## Descripción breve del proyecto
Se eligió la api [NewsAPI](https://newsapi.org/), la cual nos permite obtener miles de artículos de miles de fuentes sobre un tema en particular y en un periodo de tiempo específico.
La API cuenta con una versión gratuita que resulta suficiente para lograr el objetivo del entregable. Me pareció una opción interesante ya que los datos contienen una columna temporal que
permitiría hacer análisis en un periodo determinado de tiempo. Por otra parte contiene mucho dato *"textual"*, lo cual habilita a proyectos de *"text mining"*, en este caso de **noticias**.

Algunas consideraciones:

- Se separó el proceso de **extracción** del de **carga** de datos, esto es:
  - la creación de la tabla para DW se hizo usando *DBeaver*
  - mientras que el proceso de obtención de datos desde la API y su posterior carga se realizó con Python (librerias `requests` + `pyspark`, entre otras)