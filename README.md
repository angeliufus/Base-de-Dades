# SPRINT 1
## Exercici 1

### Hem creat una base de dades relacional amb 5 taules, he utilitzat mysql workbench.

1. **tb_genre**
* Te 5 columnes o atributs: El 1er atribut genre_id a la taula es un camp numèric que és la clau **primària** i la columna genre_name i created_by_user son camps de texte de màxim caràcters 40 i 10 respectivament, la de created_by_user te un valor per defecte (OS_SGAD), cap d'aquests 3 primers atributs son Nullable 
que vol dir que no poden contenir un valor nul, han de tenir sempre un valor o texte.
Els altres dos atributs son valors en format de data. Created_date i Updated_date i en aquest cas si que poden ser nul o sigui no tenir cap valor.
En aquesta taula simplement defineix els gèneres de pelicules.
2. **tb_movie**
* Te 8 columnes o atributs: El 1er atribut a la taula es **movie_id** que és la clau **primària** i es NOT NULL, així com la columna movie_title i created_by_user que son camps de texte de màxim caràcters 100 i 10 respectivament, la de created_by_user de nou te un valor per defecte (OS_SGAD), cap d'aquestes 3 columnes/atributs son Nullable que vol dir que no poden contenir un valor nul. Hi ha 3 columnes que son valors en format de data movie_date, created_date i updated_date i en aquest cas si que poden no contindre cap valor (son Nullable).
Aquesta taula conté un atribut **movie_genre_id** que es també un codi numèric al igual que la clau primària (movie_id) i es una clau forànea que la lliga amb la clau primària de la taula **tb_genre** 
Aquesta taula conté tots els titols de películes i la lliga amb el gènere de pelicula (Terror, comedia, etc)
3. **tb_movie_person**
* Te 7 columnes o atributs: En aquest cas te 1 atribut que es la clau **primària** (**movie_id**) i **2** que son **Foreign keys/claus forànees** (**person_id**, **role_id**) que son de nou un codi numèric que la relacionen amb altres dues taules, la taula tb_person (que conté persones de la industria del cine) i la taula tb_role (que conté els rols d'aquestes persones en les películes)
Te un atribut movie_award_ind que crec que hauria de ser booleana, ja que sembla que nomes pot ser Yes or No (0 o 1)
De nou els camps de create_date i update_date son format data i poden ser nuls, o sigui no tenir cap valor.I de nou el created_by_user te un valor pr defecte (OS_SGAD)
4. **tb_person**
*Te 9 columnes o atributs: La clau **primària** es **person_id**. Inclou el nom de les persones person_name que no pot ser NUll, el país d'on son person_country que si pot ser NULL , la data de naixement person_dob que s'ha d'entrar obligatoriament (Not Null) i la data de mort person_dod que en aquest cas si pot ser NULL ja que pot estar encara viu. 
També te un atribut que es **person_parent_id** que es una **clau forànea**, és un codi numeric que el relaciona amb la clau primària d'aquesta mateixa taula, ja que hi ha persones que son fills de altres persones de la industria del cine. En aquest cas, pot ser NULL ja que hi haurà persones que no tenen pares a la industria.
De nou els camps de create_date i update_date son format data i poden ser nuls, o sigui no tenir cap valor.I de nou el created_by_user te un valor pr defecte (OS_SGAD)
5. **tb_role**
* Te 5 columnes o atributs: La clau **primària** es **role_id**. Simplement dona un codi numéric a tots els rols que pots tenir en una película
De nou els camps de create_date i update_date son format data i poden ser nuls, o sigui no tenir cap valor.I de nou el created_by_user te un valor pr defecte (OS_SGAD)

Envio imatges a continuació del mysql workbench on he obtingut tota aquesta informació:

**Visió de les taules:**
![Visió de les taules](https://user-images.githubusercontent.com/29401511/226172135-78c7d5f8-d8d5-4222-be6a-92b984fa6da8.jpg)
**Visió del tipus d'atributs**
![Visió del tipus d'atributs](https://user-images.githubusercontent.com/29401511/226172187-46ec941d-a6f9-45b7-8af0-14e6f6f327a9.jpg)
**Visió dels arbres**
![Visió dels arbres](https://user-images.githubusercontent.com/29401511/226172197-56c26c48-577c-44e2-9899-5ee30de37a43.jpg)

## Exercici 2

Vaig a la taula tb_person i poso la query SELECT person_name, person_country, person_dob FROM movies.tb_person WHERE person_dod IS NULL ORDER BY person_dob ASC;
 I obtenim el seguent resultat:
**Visió de la query:**
![Query exercici2](https://user-images.githubusercontent.com/29401511/226173613-9e0bdb90-a064-4350-a5fc-a9c2f371da22.jpg)

## Exercici 3

Vaig a la taula tb_genre i faig la INNER JOIN seguent:
SELECT count(tb_genre.genre_name),tb_genre.genre_name FROM movies.tb_genre INNER JOIN movies.tb_movie ON tb_genre.genre_id=tb_movie.movie_genre_id
 GROUP BY movie_genre_id ORDER BY COUNT(movie_genre_id) DESC;
 **Visió de la query:**
 
![exercici 3](https://user-images.githubusercontent.com/29401511/226205387-5b5035a6-2f27-4310-a367-a90c16287418.jpg)

## Exercici 4
Vaig a la taula tb_movie_person i faig la INNER JOIN seguent:
SELECT COUNT(tb_movie_person.role_id), tb_movie_person.movie_id, tb_person.person_name 
FROM movies.tb_person INNER JOIN movies.tb_movie_person ON tb_person.person_id=tb_movie_person.person_id 
GROUP BY tb_movie_person.person_id, tb_movie_person.movie_id ORDER BY COUNT(tb_movie_person.role_id)DESC;

 **Visió de la query:**
![Exercici 4 1era part](https://user-images.githubusercontent.com/29401511/226210447-4ed85b39-8d6d-4868-a5f2-c2d87b7616df.jpg)

Per veure els que han tingut mes d'un rol en una pelicula hi afegirem la clausula HAVING :
SELECT COUNT(tb_movie_person.role_id), tb_movie_person.movie_id, tb_person.person_name 
FROM movies.tb_person INNER JOIN movies.tb_movie_person ON tb_person.person_id=tb_movie_person.person_id 
GROUP BY tb_movie_person.person_id, tb_movie_person.movie_id 
**HAVING COUNT(tb_movie_person.role_id) >1** ORDER BY COUNT(tb_movie_person.role_id) DESC;

**Visió de la query:**





