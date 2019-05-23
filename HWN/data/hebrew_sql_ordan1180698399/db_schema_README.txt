MultiWordNet SQL Export is composed of following files.


-----------------------
(1) COMMON_RELATION.SQL
-----------------------

The table "common_relation" lists all the semantic relations that are common to all languages.

Each record contains four fields:
-type: kind of relation (see below the list of MultiWordNet relations and the corresponding symbols used to codify them);
-id_source: identifier of the source synset ("pos#offset", where pos is "n" for nouns, "v" for verbs, "a" for adjectives, and "r" for adverbs);
-id_target: identifier of the target synset ("pos#offset", where pos is "n" for nouns, "v" for verbs, "a" for adjectives and "r" for adverbs);
-status: "new" if the relation involves new synsets, i.e. synsets which are not in Princeton WordNet and have been created in MultiWordNet. When the relation involves synsets which are taken from Princeton WordNet the field is "NULL".

Examples:
INSERT INTO common_relation VALUES ('*','v#00001740','v#00003763',NULL);
INSERT INTO common_relation VALUES ('@','v#00002143','v#00001740',NULL);
 	
-----------------
(2) _RELATION.SQL
-----------------

The table "_relation" contains the relations that are language dependent. These relations are instances of the standard lexical relations used in Princeton WordNet(e.g. antonymy, pertains to, etc.). Moreover, this table contains a new type of semantic relation created within MultiWordNet, which is called "nearest". The nearest relation holds between an empty synset (a lexical gap) of a certain language and the synset with the most similar meaning in that language. There are only few instances of this relation codified so far. 

Each record contains six fields:
-type: kind of relation (see below the list of MultiWordNet relations and the corresponding symbols used to codify them);
-id_source: identifier of the source synset ("pos#offset");
-id_target: identifier of the target synset ("pos#offset");
-w_source: the source lemma (only for lexical relations);
-w_target: the target lemma (only for lexical relation);
-status: "new" if this relation involves a new synset or "NULL" if it involves synsets taken from Princeton WordNet. 	

Examples:
INSERT INTO english_relation VALUES ('!','v#00009549','v#00009666','rest','be_active',NULL);
INSERT INTO italian_relation VALUES ('|','n#N0002074','n#09593084','','','new');


---------------
(3) _SYNSET.SQL
---------------

The table "_synset" contains the synsets (most of them are aligned with the Princeton WordNet but some are new ones). Also lexical gaps are specified in this file (see below).

Each record contains four fields:
-id: synset identifier ("pos#offset"). Either a Princeton WordNet identifier or a new synset identifier (beginning with "N" or "W" for Italian and English, and with "H" for Hebrew);
-word: lemmas contained in the synset, separated by a space character. The tokens of multiwords are connected by "_". The word "GAP!" is a special identifier used to describe a lexical gap;
-phrase(**): lemmas contained in the phraset, separated by a space character. The tokens of multiwords are connected by "_". 
-gloss: synsets may optionally have a gloss, composed by a definition and sometimes an  example.

Examples:
INSERT INTO english_synset VALUES ('n#00008864',' plant flora plant_life ',NULL,'a living organism lacking the power of locomotion');
INSERT INTO italian_synset VALUES ('n#00043525',' GAP! ',' errore_di_calcolo ',NULL);
INSERT INTO italian_synset VALUES ('a#02078908',' epocale ',' che_fa_epoca ','che caratterizza un\'epoca; \"una svolta epocale\"');


(**) With the term phrase we refer to a free combination of words (as opposed to lexical unit) which is currently used to express a concept. As these kinds of expressions are not lexical units, they cannot be put in the synset together with lexical units. Thus another data structure, called "Phraset", is linked to a synset and contains all the phrases related to that synset. 
As an example, the Italian free combination of words "strofinaccio dei piatti" is synonymous with the words "strofinaccio" and "canovaccio" but it is not a lexical unit. Thus, the English synset {dishrag, dishcloth} has a corresponding Italian synset {canovaccio, canavaccio, strofinaccio} which has also a phraset {strofinaccio_dei_piatti}. As another example, consider: English synset {advancement, progress}, Italian synset {avanzamento, sviluppo, progresso}, Italian phraset {passo_avanti, passo_in_avanti}. 


--------------
(4) _INDEX.SQL
--------------

The table "_index" contains the lists of the lemmas. The purpose of this table is to retrive very quickly the synset ids and the possible searches starting from a lemma in all its PoS. 

Each record contains five fields:
-lemma: contains the lemma. Multiwords are connected by "_". The word "GAP!" is a special identifier used to describe a lexical gap;
-id_n: contains the list of the synset ids (separated by a space character) in which the lemma is contained as a noun;
-id_v: contains the list of the synsets ids in which the lemma is present as a verb;
-id_a: contains the list of the synset ids in which the lemma is present as an adjective;
-id_r: contains the list of the synset ids in which the lemma is present as an adverb.

Examples:
INSERT INTO english_index VALUES ('shape','n#03952527 n#00015185 n#04055717 n#04562135 n#03685812 n#10430604 n#04554317','v#00474001 v#01139594 v#00095506','','');
INSERT INTO italian_index VALUES ('parlamentare',' n#07457674',' v#00518082','a#02590962',NULL);

--------------
(5) _SYNONYMS.SQL
--------------

The table "_synonyms" contains the lists of the synonym cards.

Each record contains five fields:
num: synonym card identifier;
lemma: contains the lemma (for Hebrew is the dotted form of the word);
pos: part of speech (could be "n" for nouns, "v" for verbs, "a" for adjectives and "r" for adverbs);
syn: synset offset;
bidict: bilingual dictionary;
bisense: sense number of the bilingual dictionary; 
monodict: monolingual dictionary;
monosense: sense number of the monolingual dictionary; 
tgr: translation group
gloss: dictionary gloss;
teqs: translational equivalents;
uso: usage (could be empty, "rare","archaic" or "regional");
ex: examples relating to the lemma;
ev: evaluation;
stat: status (could be empty, "checked" or "reviewed");
comment: comments of the authors;
history: list of the modification date and authors;
idmorpho: morphological card identifier;
modify: last update date.




--------------
(6) _MORPHO.SQL
--------------

The table "_morpho" contains the list of the morphological information.


Each record contains six fields:
id: morphological card identifier (this id is used to join the morphological information to synonym card);
lemma: contains the lemma (for Hebrew is the dotted form of the word, is the same of the synonym lemma);
pos: part of speech (could be "n" for nouns, "v" for verbs, "a" for adjectives and "r" for adverbs);
irregular_forms: useful for verbs;
pronunciation: 
miscellanea: other information like gender, number, ... (es. "f sing").

For Hebrew there are five extra fields to store several forms of the lemma:
undotted: undotted form of the lemma;
dotted_without_dots: dotted without dots form of the lemma;
variants: morphological variants;
translit_dotted: dotted transliteration of the lemma;
translit_undotted: undotted transliteration of the lemma;

Examples:
INSERT INTO hebrew_morpho VALUES (90801,'בַּיִת','n',NULL,'BAYIT','בית','בית',NULL,'BIT','BIT',NULL);




------------------
TABLE OF RELATIONS
------------------

Most of the relations are the same as in Wordnet 1.6. Only the "nearest", "composed-of" and "composes" relations have been added.
The complete list of the relations is showed below.


POS 	| type	|	description			| relation	| features | coded into DB
---------------------------------------------------------------------------------------------------------
NOUNS 	| !	| Antonyms				| antonym	| :lexical | YES
 	| @	| Hypernyms				| hypernym	|	   | YES
	| ~	| Hyponyms				| hyponym	|	   | NO (see the reverse rel @)
	| #m	| Holonyms (* is a member of)		| member-of	|	   | NO (see the reverse rel %m)
	| #s	| Holonyms (* is the substance of)	| substance-of	|	   | NO (see the reverse rel %s)
	| #p 	| Holonyms (* is a part of)		| part-of	|	   | NO (see the reverse rel %p)
	| %m	| Meronymys (members of *)		| has-member	|	   | YES
	| %s 	| Meronyms (substances of *)		| has-substance	|	   | YES
	| %p	| Meronymys (parts of *)		| has-part	|	   | YES
	| =	| Attributes (is a value of *)		| attribute	|	   | YES
	| |	| Synset nearest to *			| nearest	|	   | YES
	| +c	| Composed-of (is composed of *)	| composed-of	| :lexical | YES
	| -c	| Composes (composes of *)		| composes	| :lexical | NO (see the reverse rel +c)

VERBS	| !	| Antonyms				| antonym	| :lexical | YES
	| @	| Hypernyms				| hypernym	|	   | YES
	| ~ 	| Hyponyms				| hyponym	|	   | NO (see the reverse rel @)
	| *  	| Entails doing				| entailment	|	   | YES
	| >	| Causes				| causes	|	   | YES
	| ^ 	| Also see				| also-see	|	   | YES
	| $  	| senses of * grouped by similarity	| verb-group	| new 1.6  | YES
	| |	| Synset nearest to *			| nearest	|	   | YES
	| +c	| Composed-of (is composed of *)	| composed-of	| :lexical | YES
	| -c	| Composes (composes of *)		| composes	| :lexical | NO (see the reverse rel +c)

ADJ	| !	| Antonyms				| antonym	| :lexical | YES
	| & 	| Similar to				| similar-to	|	   | YES
	| <	| Participle of verb			| participle	| :lexical | YES
 	| \	| Pertains to noun			| pertains-to	| :lexical | YES
	| =	| Value of (* is a value of)		| is-value-of	|	   | YES
	| ^	| Also see				| also-see	|	   | YES
	| |	| Synset nearest to *			| nearest	|	   | YES
	| +c	| Composed-of (is composed of *)	| composed-of	| :lexical | YES
	| -c	| Composes (composes of *)		| composes	| :lexical | NO (see the reverse rel +c)

ADV	| !	| Antonyms				| antonym	| :lexical | YES
	| \     | Derived from adjective		| derived-from	|	   | YES
	| |     | Synset nearest to *			| nearest	|	   | YES
	| +c	| Composed-of (is composed of *)	| composed-of	| :lexical | YES
	| -c	| Composes (composes of *)		| composes	| :lexical | NO (see the reverse rel +c)



