# Update to do!!!
mapping_exhib_section.x3ml
mapping_personage.x3ml
mapping_works.x3ml


# Table of contents
- [Introduction](#introduction)
	- [Context](#context)
	- [Graphical representation](#graphical-representation)
- [Main entities](#main-entities)
	- [address + city + country](#address-+-city-+-country)
	- [article](#article)
	- [catalog](#catalog)
	- [committee](#committee)
	- [exhibition](#exhibition)
	- [exhibition group](#exhibition-group)
	- [exhibition section](#exhibition-section)
	- [personage](#personage)
	- [work](#work)
- [Note on temporal dimensions](#Note-on-temporal-dimensions)

# Introduction
## Context
The [Art@s](https://artlas.huma-num.fr/en/about/) project, founded and directed by the Professor Béatrice Joyeux-Prunel, had produced [BasArt](https://artlas.huma-num.fr/en/artlas-bases-de-donnees-en-acces-public/), an Open Access and collaborative international global database of exhibition catalogues from the 19th century to the present
day.
This data model has been created to transform the original SQL database of BasArt into a turtle triple store.
The data model has been developed using a bottom-up approach, in order to be easily adapted to the actual data one can find in exhibition catalogues, our information sources.

The model uses [CIDOC CRM](https://www.cidoc-crm.org/) ontology. It was based on the [7.1 Version of the Class & Properties declaration](https://www.cidoc-crm.org/Version/version-7.1). All the versions are available [here](https://www.cidoc-crm.org/versions-of-the-cidoc-crm).
To link an entity with its corresponding wikidata entity, the model use the property crmdig:L54_is_same-as, coming from the [CRM Digital](https://cidoc-crm.org/crmdig/) extension of CIDOC CRM. The complete version list of its Class & Properties is available [here](https://cidoc-crm.org/crmdig/fm_releases). In some specific cases, other class and properties are used. These elements come from the [SARI (Swiss Art Research Infrastructure) Reference Models](https://docs.swissartresearch.net/) and a new property, temporarily named basart:is_advisor, has been proposed.
Many entities are linked to an entry in [Getty AAT Thesaurus](https://www.getty.edu/research/tools/vocabularies/aat/) in order to add precision about their type.

## Graphical representation
All the graphical representations of the data model were made on draw.io.

Each page is centred on one of the main entities of the model.
To simplify the understanding of the schema, entities are always linked from top to bottom, forming level of depth. These levels don't bear any significance, they just somehow represent the maximal distance between the top entity and its related entities. In some cases, properties form "shortcut" between the top entity and another one, situated many level below.

**Squares with rounded corner** (thereafter "bubbles") represent **Entities** with their own URI.
**Arrows** represent **Predicates**.
**Squares with right angles** represent **Literals**, with an example drawn from the Basart database, written between quotes. In some cases, no examples were available that display an information for every field of information conceived in the mapping. In these cases, the graphical representation show a square with right angles containing only empty quotes, in order to display all the information fields available in the model, but the automatically generated turtle files won't create any empty nodes.

The color code and shapes for the entities come from a draw.io library, available on [GitHub](https://github.com/ncarboni/Shapes_CIDOC-CRM).

The E55_Type entities are a bit particular, because they are either composed of a fixed URI for every entity of a given type to which they are related (e.g. every city is linked to the E55_Type "cities", that has the fixed URI <http://vocab.getty.edu/aat/300008389>), or of one URI among a fixed list, as for a language (e.g. every English article is linked to the fixed URI http://vocab.getty.edu/aat/300388277, while every French article is linked to the fixed URI http://vocab.getty.edu/aat/300388306). In both these cases, the entities E55_Type will be graphically represented as an orange bubble indicating the URI (or an example of URI) between angle brackets, followed by the literal content between quotes.

# Main entities
## address + city + country
They are all **instanced as CRM entities E53_Place**, but they are linked to different E55_Types, each referring to a different entry in the Getty
That page is the only one in the graphical representation of the model that regroups 3 main entities in one: **Address**, **City** and **Country**.
As these entities are closely linked, it is clearer to represent them on the same schema.

There are no link from these entities that reach any other main entity.

## article
Articles are **instanced as both E31_Document and E33_Linguistic_Object**, in order to allow them both the properties P106i_forms_part_of (a catalog) and P72_has_language.
Articles are important in this model because they are the source from which information has been taken. They are useful to cite the sources and their authors.

Articles may be linked to the following main entities:
- catalog (from which they are extracted)
- personage (their authors)

## catalog
Catalogs are **instanced as both E31_Document and E33_Linguistic_Object**, in order to allow them both the properties P129_is_about (an exhibition) and P72_has_language.
Catalogs are important in this model because they are the main source of information from which all the data has been taken.
More details are explained for the catalogs than for the articles, in particular the information concerning their edition and the exhibition that they document.

Catalogs may be linked to the following main entities:
- city (Place of publication)
- exhibition (that they documents)
- personage (their editors)
- committee (responsible for their conceptual creation)

## committee
Committees are **instanced as E39_Actor**.
Committees represent the authorities in charge of conceptualising the catalogs. Given the nature of the available data in BasArt (and thus in the sources that were used to build the database) about them, they couldn't be expressed in the same way as author personages are.

There are no link from these entities that reach any other main entity.

## exhibition
Exhibitions are **instanced as E7_Activity**.
Exhibitions are the core events of the model.

Exhibition may be linked to the following main entities:
- city
- exhibition group
- source (if there is no catalog referring to the exhibition)

## exhibition group
Exhibition groups are **instanced as E39_Actor**.
Exhibition groups are the actors or groups of actors that put up the exhibition.
Theses entities just have indication about their identifier, name, and group type.
The group types are indicated by specific resources that we have built ourselves.

There are no link from these entities that reach any other main entity.

## exhibition section
Exhibition sections are **instanced as E7_Activity**.
Exhibition sections are parts of an exhibition. As such, they are more detailed than the global exhibition from which they form a part. These details may be about a more precise delimitation in space and/or time, about their title, or about the nature of the institution that hosted the event.

Whereas exhibitions only indicate a city as a location, exhibition sections may indicate a precise address. Given the complexity of addresses data, an exhibition section may be linked to an address defined as a structured entry, and/or also to another address defined as plain text.
The section appellation may be composed of a title and an hosting space name, separated by a comma. It may also be composed of only one of those two elements.

Exhibition sections may be linked to the following main entities:
- exhibition
- catalog
- address
- committee

## personage
Personages are **instanced as E39_Actor**.
These entities are used to express artists, articles authors and catalog editors.
Their first name and last name are separated, enabling researcher to easily understand which is what. The personage's label combine both, separated by a blank space.
Their nationality and membership to an artists group or school is expressed by a link to an E74_Group entity. No preceding lists of these nationalities or memberships were available to create the corresponding entities. There are thus generated on the fly, with an URI using a hash of their literal content.
Their birth and date have their own nodes, that may both be linked to an address and date.

Personages may be linked to the following main entities:
- address (of birth and / or death, of living and / or working place)
- another personage (instructor)

## work
They are **instanced as E22_Human-Made_Object**.
Works are the central and most numerous entities of the model. Their part of the model is a bit more complex than others. The major part of the data can be generated in turtle triples by using a single couple of xml / x3ml files. But, in order to allow links from one work to multiple categories and medium, complementary couples of xml /x3ml files have to be used.
No preceding lists of medium or categories were available to create the corresponding entities. There are thus generated on the fly, with an URI using a hash of their literal content.

The links between a work and the medium used to create it pass by an E12_Production entity. It is then linked with a P33_used_specific_technique property. We chose this property instead of P32_used_general_technique, because P33 allows the use of Literal, while P32 is stricter and demands the use of a controlled language.

Works titles are a bit complex. It is composed of 3 parts: main title, subtitle and title translation. A 4th entity is created to indicate the full title, that regroups the main title and the subtitle, separated by a semi-colon. The latter is a translation of both the main title and the subtitle when available. The work label is only composed of the main title.
While the main title and subtitle are instanced only as E41_Appellation, the full title and its translation have to be doubled instanced as E41_Appellation and E33_Linguistic_Object, in order to allow linking them with the P73_has_translation property.
The various link between these 4 entities and the work entity makes the mapping file difficult to read. It is then best to read it aside with its global overview given in the graphical representation.

Expressing if a given work is reproduced or not in the catalog that mention it is a bit tricky, as semantic web wasn't designed to express negative information. We thus choose to express it by a complementary E55_Type entity, that can be either "work_image_in_catalog" or "no_work_image_in_catalog". If the information is missing, no complementary entity will be created.

Because of the data nature we had about work dimensions, we chose to express it as a single literal, even if it doesn't allow great analysis of these information.

Some images of works were present in the catalog and were then digitalized. It would be really interesting to link these image files to their corresponding work entities. Also, data about the work owner, the work price, the catalog chapter or pages in which it is documented, the room of the institution in which it was displayed and complementary notes were available. Unfortunately, I wasn't able to map all these information in the limited time of my internship. A complementary work could be done to integrate these data that are quite frequently available in catalogs.

Work mediums (techniques used in their creation, e.g. *painting*) and categories (the nature of the work, e.g. *paintings*) may be plural. For this reason, they are both mapped in a separate mapping file, that can handle this multiplicity. For the time being, each medium and category has a URI built using a hash of their literal content. A great upgrade of the mapping would be to link these technique to their corresponding Getty AAT entries. Unfortunately, I wasn't able to complete this task during the limited time of my internship.

# Note on temporal dimensions
The temporal dimension are always expressed as E52_Time-Span, composed of a beginning date and an ending date. Date are given in the xsd:dateTime format 
"1985-01-01T00:00:00"^^xsd:dateTime
Dates are as precise as possible. In case of approximate date (e.g. only the year), minimal values are added for the beginning dates and maximal values for the ending dates. In case only one date is available for a given event, the minimal and maximal values are added to express beginning and ending date. (e.g. "1985" becomes `"1985-01-01T00:00:00"^^xsd:dateTime` for the beginning and `"1985-12-31T23:59:59"^^xsd:dateTime` for the ending date.)
Even though their informational content is of utmost importance, they aren't considered among the 'Main entities' here, because they're always composed of mere Literals.