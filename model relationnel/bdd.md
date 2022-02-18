### Première étape : Traduction des entités
Les entités se traduisent par :
* Client(++IdClient++, NomClient, Prenom, Genre, DDN, PcodeClient, Score)
* Évènement(++IdEv++, Niveau, Date)
* Sport(++IdSport++, NomSport, NbMin NbMax)
* Lieu(++IdLieu++, Intitulé, PCodeLieu, Capacité)
* TerrainSalle(Surface, Type, ++IdTer++)

### Deuxième étape : Traduction des relations
Les relations se traduisent par :
* R_Participe(++IdClient++, ++IdEv ^Not Null++)
* R_Participe(IdClient)⊆Client(IdClient)
* R_Participe(IdEv)⊆Évènement(IdEv)
* R_Organise(++IdClient++, ++IdEv ^Not Null++)
* R_Organise(IdClient)⊆Client(IdClient)
* R_Organise(IdEv)⊆Évènement(IdEv)
* R_Cible(++IdEv ^Not null++, ++IdSport++)
* R_Cible(IdEv)⊆Évènement(IdEv)
* R_Cible(IdSport)⊆Sport(IdSport)
* R_Se_situe(++IdEv ^Not null++, ++IdLieu++)
* R_Se_situe(IdEv)⊆Évènement(IdEv)
* R_Se_situe(IdLieu)⊆Lieu(IdLieu)
* R_Se_pratique(++IdSport ^Not null++, ++IdTer ^Not null++)
* R_Se_pratique(IdSport)⊆R_Sport(IdSport)
* R_Se_pratique(IdTer)⊆R_TerrainSalle(IdTer)
* R_Est_équipé(++IdTer ^Not null++, ++IdLieu ^Not null++)
* R_Est_équipé(IdLieu)⊆Lieu(IdLieu)
* R_Est_équipé(IdTer)⊆TerrainSalle(IdTer)

### Troisième étape : Raffinement de la traduction
En optimisant les relations et incluant les contraintes de cardinalité et d'entités faibles, on a :

* R_Client(++IdClient++, NomClient, Prenom, Genre, DDN, PcodeClient, Score)
* R_Évènement(++IdEv++, Niveau, Date, **++IdSport ^Not null++**, **++IdClient^Organisateur ^Not null++**, **++IdLieu++ ^Not null**)
* R_Évènement(IdClient)⊆Client(IdClient)
* R_Évènement(IdSport)⊆Sport(IdSport)
* R_Évènement(IdLieu)⊆Lieu(IdLieu)
* R_Sport(++IdSport++, NomSport, NbMin NbMax)
* R_Lieu(++IdLieu++, Intitulé, PCodeLieu, Capacité)
* R_TerrainSalle(Surface, Type, ++IdTer++, **++IdLieu++ ^Not null**, **++IdSport++ ^Not null**) ^pour weak
* R_TerrainSalle(IdSport)⊆R_Sport(IdSport)
* R_TerrainSalle(IdLieu)⊆Lieu(IdLieu)
* R_Participe(IdClient, IdEv ^Not null)
* R_Participe(IdClient)⊆Client(IdClient)
* R_Participe(IdEv)⊆Évènement(IdEv) 

Les ajouts faits sont :
* Indication de R_TerrainSalle comme entité faible rattachée à R_Lieu
* Indication des contraintes de cardinalité
* Fusion de R_Cible (par rapport à R_Sport), R_Organise (par rapport à R_Client), et R_Se_situe (par rapport à R_Lieu) dans R_Évènement
* Fusion de R_Se_pratique (par rapport à R_Sport) dans R_TerrainSalle
