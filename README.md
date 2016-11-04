# TP3-NF16
Magasin avec Rayons et Produits (listes chainées)

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

/*typedef struct Listerayon Listerayon;
 struct Listerayon
 {
 Listerayon *premierrayon;
 };
 
 typedef struct Listeproduit Listeproduit;
 struct Listeproduit
 {
 Listeproduit *premierproduit;
 };
 
 typedef struct Listemag Listemag;
 struct Listemag
 {
 Listemag *premiermag;
 };*/

// les struct au dessus servent à avoir le premier de chaque liste mais on en a pas besoin ici je crois

struct Produit{
    char marque[50];
    float prix;
    char qualite;
    int quantite_en_stock;
    struct Produit *suivant;
};
typedef struct Produit produit;

struct Rayon{
    char nom_rayon[50];
    int nombre_produit;
    produit *premier;
    struct Rayon *suivant;
};
typedef struct Rayon rayon;

struct Magasin{
    char nom[50];
    rayon *premier;
};
typedef struct Magasin magasin;

produit *creerProduit(char *marque, float prix, char qualite, int quantite){
    produit *produit=malloc(sizeof(produit));
    strcpy(produit -> marque,marque); //est ce qu'on met *marque?
    produit -> prix=prix;
    produit -> qualite=qualite;
    produit -> quantite_en_stock=quantite;
    return produit;
}

rayon *creerRayon(char *nom, int n){
    rayon *ray=malloc(sizeof(rayon));
    strcpy(ray -> nom_rayon,nom);
    ray-> nombre_produit=n;
    return ray;
}

magasin *creerMagasin(char *nom){
    magasin *magasin=malloc(sizeof(magasin));
    strcpy(magasin -> nom, nom);
    return magasin;
}

/*magasin *initialisationrayon()
{
    magasin *magasin = malloc(sizeof(magasin));
    rayon *rayon = malloc(sizeof(rayon));
    if(magasin == NULL || rayon == NULL)
    {
        exit(EXIT_FAILURE);
    }
    
    strcpy(rayon->nom_rayon, '\0');
    rayon->nombre_produit=0;
    rayon->premier = NULL;
    rayon->suivant = NULL;
    magasin->premier = rayon;
    
    return magasin;
}*/



int ajouterRayon(magasin *magasin, rayon *rayon){ // JULIE on ne teste pas si le rayon existe déjà je crois
    struct Rayon *nouveau = malloc(sizeof(struct Rayon));
    struct Rayon *temp = malloc(sizeof(struct Rayon));
    struct Rayon *test = malloc(sizeof(struct Rayon));
    char *str1, *str2;
    if (magasin == NULL || rayon == NULL || nouveau == NULL || temp == NULL ||test == NULL) //si magasin->premier null alors faire pointer magasin->premier sur rayon
    {
        exit(EXIT_FAILURE);
    }
    
    strcpy(nouveau->nom_rayon,rayon -> nom_rayon); //nouveau = rayon ajouté
    
    test = magasin -> premier;
    /*while (test!=NULL)
    {
        if (test == rayon)
        {
            printf("Le rayon existe déjà\n");
            return 0;
        }
        test = test->suivant;
    }*/
    
    str1 = nouveau-> nom_rayon;
    str2 = magasin->premier->nom_rayon;
    
    if ((strcmp(nouveau->nom_rayon,magasin->premier->nom_rayon)<=0 )||(magasin->premier->nom_rayon == NULL))
        //nom_rayon[0]= premiere lettre du nom du rayon
    {
        nouveau->suivant = magasin->premier;
        magasin->premier = nouveau;
    }
    else
    {
        temp = magasin -> premier;
        while ((nouveau -> nom_rayon[0] >= temp -> nom_rayon[0]) && (temp !=NULL))
        {
            temp = temp->suivant;
        }
        if (temp == NULL)
        {
            temp = nouveau;
        }
        else
        {
            nouveau->suivant = temp;
            temp = nouveau;
        }
    }
    test = magasin -> premier;
    while (test!=NULL)
    {
        if (test == rayon) // JULIE pas compris le test == rayon ? test si le rayon est bien inseré ?
            return 1;
        test = test->suivant;
    }
    return 0;
    
}

int ajouterProduit(rayon *rayon, produit *produit)
{                                                                       // JULIE on ne teste pas si le rayon existe déjà je crois
    struct Produit *nouveau = malloc(sizeof(struct Produit));
    struct Produit *temp = malloc(sizeof(struct Produit));
    struct Produit *test = malloc(sizeof(struct Produit));
    if (rayon == NULL || produit == NULL || nouveau == NULL || temp == NULL ||test == NULL)
    {
        printf("erreur");                                                       // discuter difference (avantages/inconvenients) entre return 0 et exit (exit_failure) dans le rapport
        return 0;
    }
    
    nouveau->prix = produit -> prix; //nouveau = rayon ajouté
    
    test = rayon-> premier;
    while (test!=NULL)
    {
        if (test == produit)
        {
            printf("Le produit existe déjà\n");
            return 0;
        }
        test = test->suivant;
    }
    
    if (nouveau->prix <= rayon->premier->prix)
    {
        nouveau->suivant = rayon->premier;
        rayon->premier = nouveau;
    }
    else
    {
        temp = rayon -> premier;
        while ((nouveau -> prix >= temp -> prix) && (temp !=NULL))
        {
            temp = temp->suivant;
        }
        if (temp == NULL)
        {
            temp = nouveau;
        }
        else
        {
            nouveau->suivant = temp;
            temp = nouveau;
        }
    }
    test = rayon -> premier;
    while (test!=NULL)
    {
        if (test == produit)
            return 1;
        test = test->suivant;
    }
    return 0;
    
}


void afficherMagasin(magasin *magasin)
{
    if (magasin == NULL)
    {
        printf("erreur");
        exit(EXIT_FAILURE);
    }
    
    struct Magasin *actuel = malloc(sizeof(struct Magasin));
    struct Rayon *actuelR = malloc(sizeof(struct Rayon));
    
    if (actuel == NULL ||actuelR == NULL)
    {
        printf("erreur");
        exit(EXIT_FAILURE);
    }
    
    actuel->premier=magasin->premier;
    actuelR = actuel -> premier;
    
    printf("Nom\t:\tNombre de produits\n");
    
    while (actuelR != NULL)
    {
        printf("%s\t:\t%d", actuelR->nom_rayon, actuelR->nombre_produit);
        actuelR = actuelR->suivant;
    }
}

void afficherRayon(rayon *rayon)
{
    if (rayon == NULL)
    {
        printf("erreur");
        exit(EXIT_FAILURE);
    }
    
    struct Rayon *actuel = malloc(sizeof(struct Rayon));
    struct Produit *actuelR = malloc(sizeof(struct Produit));
    
    if (actuel == NULL ||actuelR == NULL)
    {
        printf("erreur");
        exit(EXIT_FAILURE);
    }
    
    actuel->premier=rayon->premier;
    actuelR = actuel -> premier;
    
    printf("Marque\t:Prix\t:Qualite\t:Quantité en stock\n");
    
    while (actuelR != NULL)
    {
        printf("%s\t:%f\t:%c\t:%d\t\n", actuelR->marque, actuelR->prix, actuelR->qualite, actuelR->quantite_en_stock );
        actuelR = actuelR->suivant;
    }
}

int supprimerProduit(rayon *rayon, char* marque_produit)
{
    if (rayon == NULL)                                              // si rayon n'existe pas erreur
    {
        printf("ce rayon n'existe pas");
        return 0;
    }
    
    struct Produit *actuel = malloc(sizeof(struct Produit));        // creation d'un pointeur sur le produit en cours d'analyse
    struct Produit *prec = malloc(sizeof(struct Produit));          //creation d'un pointeur sur le produit précedent le produit actuel
    struct Produit *test = malloc(sizeof(struct Produit));
    
    if (actuel == NULL)
    {
        return 0;
    }
    
    actuel = rayon->premier;                                        // actuel prend le premier produit du rayon
    
    while((actuel->marque != marque_produit) || (actuel == NULL))   //on parcours le rayon tant que la marque du produit analysé est differente de la marque rentrée en paramètre ou que le produit est vide
    {
        actuel=actuel->suivant;                                     //on passe au produit suivant du rayon
        prec->suivant=actuel;                                       // l'ancien actuel devient le precedent
    }
    
    if (actuel==NULL)                                               //si on sort du while en fin de rayon , c'est un echec on renvoi 0
    {
        printf("le produit n'est pas dans ce rayon");
        return 0;
    }
    
    else
    {
        prec->suivant=actuel->suivant;                              //pour supprimer l'actuel on fait pointer le suivant du precedent sur le suivant de l'actuel.
        free (actuel);                                              //on libère la mémoire allouer au pointeur actuel
    }
    
    test = rayon-> premier;                                         //test repart au debut du rayon
    while (test!=NULL)                                              //on passe tout le rayon en revue pour verifier que le produit est bien supprimé
    {
        if (test->marque == marque_produit )
        {
            printf("le produit n'a pas été supprimé");
            return 0;
        }
        test = test->suivant;
    }
    return 1;
    
    
}

int supprimerRayon(magasin *magasin, char *nomdurayon)
{
    if (magasin == NULL)                                              // si rayon n'existe pas erreur
    {
        printf("ce magasin n'existe pas");
        return 0;
    }
    
    struct Rayon *actuel = malloc(sizeof(struct Rayon));        // creation d'un pointeur sur le produit en cours d'analyse
    struct Rayon *prec = malloc(sizeof(struct Rayon));          //creation d'un pointeur sur le produit précedent le produit actuel
    struct Rayon *test = malloc(sizeof(struct Rayon));
    
    if (actuel == NULL)
    {
        return 0;
    }
    
    actuel = magasin->premier;                                      // actuel prend le premier produit du rayon
    prec = magasin->premier;
    
    while((actuel->nom_rayon != nomdurayon) || (actuel == NULL))   //on parcours le rayon tant que la marque du produit analysé est differente de la marque rentrée en paramètre ou que le produit est vide
    {
        actuel=actuel->suivant;                                     //on passe au produit suivant du rayon
        prec->suivant=actuel;                                       // l'ancien actuel devient le precedent
    }
    
    if (actuel==NULL)                                               //si on sort du while en fin de rayon , c'est un echec on renvoi 0
    {
        printf("le rayon n'est pas dans ce magasin");
        return 0;
    }
    
    else
    {
        prec->suivant=actuel->suivant;                              //pour supprimer l'actuel on fait pointer le suivant du precedent sur le suivant de l'actuel.
        free (actuel);                                              //on libère la mémoire allouer au pointeur actuel
    }
    
    test = magasin-> premier;                                         //test repart au debut du rayon
    while (test!=NULL)                                              //on passe tout le rayon en revue pour verifier que le produit est bien supprimé
    {
        if (test->nom_rayon == nomdurayon )
        {
            printf("le rayon n'a pas été supprimé");
            return 0;
        }
        test = test->suivant;
    }
    return 1;
    
    
}

/*1. Créer un magasin
 2. Ajouter un rayon au magasin
 3. Ajouter un produit dans un rayon
 4. Afficher les rayons du magasin
 5. Afficher les produits d'un rayon
 6. Supprimer un produit
 7. Supprimer un rayon
 8. Rechercher un produit par prix
 9. Quitter*/

int main(int argc, const char * argv[])
{
    rayon *rayon1;
    magasin *mag;
    //int choix;
    
    //printf("Bonjour petit Humain, Bienvenue dans l'atelier de création JuLaure ! \nRegarde tout ce que tu peux faire : \n 1- Créer ton propre magasin ")
    
    mag=creerMagasin("julaure");
    mag=initialisationrayon();
    rayon1=creerRayon("Bonbons", 5);
    ajouterRayon(mag, rayon1);
    afficherMagasin(mag);
    
    
    return 0;
}
