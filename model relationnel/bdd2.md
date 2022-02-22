### Première étape : Traduction des entités
Les entités se traduisent par :
* Client(++IdClient++, NomClient, Prénom, Genre, DDN, PcodeClient, Score)
* Évènement(++IdEv++, Niveau, Date)
* Sport(++IdSport++, NomSport, NbMin NbMax)
* Lieu(++IdLieu++, Intitulé, PCodeLieu, Capacité)
* TerrainSalle(Surface, Type, ++IdTer++)

### Traduction des relations
Les relations se traduisent par :
* R_Participe(++IdClient++, ++IdEv++ not null)
* R_Participe(IdClient)⊆Client(IdClient)
* R_Participe(IdEv)⊆Évènement(IdEv)
* R_Organise(++IdClient++, ++IdEv++ not null)
* R_Organise(IdClient)⊆Client(IdClient)
* R_Organise(IdEv)⊆Évènement(IdEv)
* R_Cible(++IdEv++ not null, ++IdSport++)
* R_Cible(IdEv)⊆Évènement(IdEv)
* R_Cible(IdSport)⊆Sport(IdSport)
* R_Se_situe(++IdEv++ not null, ++IdLieu++)
* R_Se_situe(IdEv)⊆Évènement(IdEv)
* R_Se_situe(IdLieu)⊆Lieu(IdLieu)
* R_Se_pratique(++IdSport++ not null, ++IdTer++ not null)
* R_Se_pratique(IdSport)⊆R_Sport(IdSport)
* R_Se_pratique(IdTer)⊆R_TerrainSalle(IdTer)
* R_Est_équipé(++IdTer++ not null, ++IdLieu++ not null)
* R_Est_équipé(IdLieu)⊆Lieu(IdLieu)
* R_Est_équipé(IdTer)⊆TerrainSalle(IdTer)

### Deuxième étape : Instance de la base de donnée (exemple)
Client :
| IdClient | NomClient | Prénom | Genre | DDN | PcodeClient | Score |
| --- | --- | --- | --- | --- | --- | --- |
| 001 | Dupont | Jean | M | 19960427 | 91630 | 7 |
| 002 | Adjani | Isabelle | F | 19550627 | 75001 | 10 |
| 003 | Martin | Pierre | M | 19840109 | 91460 | 4 |
| 004 | Martin | Marianne | F | 17890827 | 91100 | 6 |

Évènement :
| IdEv | Niveau | Date |
| --- | --- | --- |
| 001 | Débutant | 20220220 |
| 002 | Confirmé | 20220301 |

Sport :
| IdSport | NomSport | NbMin | NbMax |
| --- | --- | --- | --- |
| 001 | Football | 22 | 22 |
| 002 | Tennis | 2 | 4 |

Lieu :
| IdLieu | Intitulé | PCodeLieu | Capacité |
| --- | --- | --- | --- |
| 001 | Orsay | 91400 | 100 |
| 002 | Bures | 91440 | 50 |

TerrainSalle :
| IdTer | Surface | Type |
| --- | --- | --- |
| 001 | 100 | Extérieur |
| 002 | 50 | Extérieur |
| 003 | 50 | Intérieur |


^
Participe
| IdClient | IdEv |
| --- | --- | 
| 001 | 001 | 
| 002 | 001 | 
| 001 | 002 | 

Organise
| IdClient | IdEv |
| --- | --- | 
| 002 | 001 | 
| 002 | 002 | 

Cible
| idEv | idSport|
| --- | --- | 
| 001 | 001 | 
| 002 | 001 | 


Se_situe
| idEv | idLieu|
| --- | --- | 
| 001 | 002 | 
| 002 | 002 | 

Se_Pratique
| idSport | idTer |
| --- | --- | 
| 001 | 001 | 
| 002 | 002 | 
| 002 | 003 | 

Est_equipe
| idTer nn| idLieu nn|
| --- | --- | 
| 001 | 001 | 
| 002 | 002 | 
| 003 | 001 | 

^


 

### Troisième étape : Raffinement de la traduction
En optimisant les relations et incluant les contraintes de cardinalité et d'entités faibles, on a :

* R_Client(++IdClient++, NomClient, Prenom, Genre, DDN, PcodeClient, Score)
* R_Évènement(++IdEv++, Niveau, Date, **++IdSport++ not null**, **++IdClientOrganisateur++ not null**, **++IdLieu++ Not null**)
* R_Évènement(IdClient)⊆Client(IdClient)
* R_Évènement(IdSport)⊆Sport(IdSport)
* R_Évènement(IdLieu)⊆Lieu(IdLieu)
* R_Sport(++IdSport++, NomSport, NbMin NbMax)
* R_Lieu(++IdLieu++, Intitulé, PCodeLieu, Capacité)
* R_TerrainSalle(Surface, Type, ++IdTer++, **++IdLieu++ not null**, **++IdSport++ not null**) pour weak
* R_TerrainSalle(IdSport)⊆R_Sport(IdSport)
* R_TerrainSalle(IdLieu)⊆Lieu(IdLieu)
* R_Participe(IdClient, IdEv not null)
* R_Participe(IdClient)⊆Client(IdClient)
* R_Participe(IdEv)⊆Évènement(IdEv) 

Les ajouts faits sont :
* Indication de R_TerrainSalle comme entité faible rattachée à R_Lieu
* Indication des contraintes de cardinalité
* Fusion de R_Cible (par rapport à R_Sport), R_Organise (par rapport à R_Client), et R_Se_situe (par rapport à R_Lieu) dans R_Évènement
* Fusion de R_Se_pratique (par rapport à R_Sport) dans R_TerrainSalle

#### Exemples

Évènement :
| IdEv | Lieu | Date | IdSport | IdClientOrganisateur | IdLieu |
| --- | --- | --- | --- | --- | --- |
| 001 | Orsay | 20220220 | 001 | 003 | 001 |
| 002 | Bures | 20220301 | 002 | 001 | 002 |

TerrainSalle :
| IdTer | Surface | Type | IdLieu | IdSport |
| --- | --- | --- | --- | --- |
| 001 | 100 | Extérieur | 001 | 001 |
| 002 | 50 | Extérieur | 002 | 002 |
| 003 | 50 | Intérieur | 002 | 002 |

