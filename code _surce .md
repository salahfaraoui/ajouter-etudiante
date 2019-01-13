#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Etudiant{
char *Nom;
char *Prenom;
float Note;
struct Etudiant *suivant;
}Etudiant;

typedef struct LSC{
Etudiant *debut;
int NB_etudiant;
}LSC;
void initialiser(LSC *l){
l->debut=NULL;
l->NB_etudiant=0;
}

Etudiant* Allouer(char *N,char *P,float note){
Etudiant *NE=(Etudiant*)malloc(sizeof(Etudiant));

if(NE==NULL){return NULL;}
else{
if((NE->Nom=(char*)malloc(sizeof(char)*30))==NULL) return NULL;
if((NE->Prenom=(char*)malloc(sizeof(char)*30))==NULL) return NULL;

strcpy(NE->Nom,N);
strcpy(NE->Prenom,P);
NE->Note=note;
NE->suivant=NULL;
return NE;
}
}

void ajouter_debut(LSC *l,Etudiant *E){
E->suivant=l->debut;
l->debut=E;
l->NB_etudiant++;
}

void ajouter_fin(LSC *l,Etudiant *E){
Etudiant *c=l->debut;
if(l->debut==NULL) l->debut=E;
else{
while(c->suivant!=NULL){
c=c->suivant;
}
c->suivant=E;
}
l->NB_etudiant++;
}
void liberer(LSC *l){
Etudiant *supp;
while(l->debut!=NULL){
supp=l->debut;
l->debut=l->debut->suivant;
free(supp->Nom);
free(supp->Prenom);
free(supp);
}
}
void diviser_version1(LSC l,LSC *LA,LSC *LNA){
Etudiant *c=l.debut;
while(c!=NULL){
Etudiant *NE=Allouer(c->Nom,c->Prenom,c->Note);
if(NE->Note>=10){ajouter_debut(LA,NE);}
else{ajouter_debut(LNA,NE);}
c=c->suivant;
}
}
void permuter(Etudiant *E1,Etudiant *E2){
Etudiant p;
p.Nom=(char*)malloc(sizeof(char)*30);
p.Prenom=(char*)malloc(sizeof(char)*30);

strcpy(p.Nom,E1->Nom);
strcpy(p.Prenom,E1->Prenom);
p.Note=E1->Note;

strcpy(E1->Nom,E2->Nom);
strcpy(E1->Prenom,E2->Prenom);
E1->Note=E2->Note;

strcpy(E2->Nom,p.Nom);
strcpy(E2->Prenom,p.Prenom);
E2->Note=p.Note;
}
void afficher(LSC l){
Etudiant *c=l.debut;
int i=1;
while(c!=NULL){
printf(“Etudiant %d : %s \t %s \t %.2f \n”,i,c->Nom,c->Prenom,c->Note);
c=c->suivant;
i++;
}
}
void trier(LSC *l){
Etudiant *i=l->debut,*j;
while(i->suivant!=NULL){
j=i->suivant;
while(j!=NULL){
if(i->Note < j->Note){
permuter(i,j);
}
j=j->suivant;
}
i=i->suivant;
}
}
int main()
{
LSC liste,LA,LNA;
initialiser(&liste);
initialiser(&LA);
initialiser(&LNA);

ajouter_debut(&liste,Allouer(“ERROUIDI”,”Mohamed”,13));
ajouter_debut(&liste,Allouer(“Amin”,”Kamal”,11));
ajouter_debut(&liste,Allouer(“Moudni”,”Alae”,16));
ajouter_fin(&liste,Allouer(“karimi”,”youssef”,8));
diviser_version1(liste,&LA,&LNA);

printf(“\nLa liste pricipale des etudiants : \n”);
afficher(liste);
printf(“\nLa liste des etudiants Admis : \n”);
afficher(LA);
printf(“\nLa liste des etudiants Non Admis : \n”);
afficher(LNA);

trier(&LA);
printf(“\nLa liste des etudiants Admis tries oar order de merite : \n”);
afficher(LA);
return 0;
}
