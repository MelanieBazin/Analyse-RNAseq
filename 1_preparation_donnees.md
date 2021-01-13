---
title: Préparation des données RNA-seq
author: Pierre Poulain
license: Creative Commons Attribution-ShareAlike (CC BY-SA 4.0)
---

L'objectif de cette partie est de télécharger les données RNA-seq de *O. tauri* nécessaires à l'analyse.

Le jeu de données initial est consituté de 47 fichiers *.fastq.gz* (format *.fastq* compressé) pour un total de 24 Go.

Pour que cette activité se déroule dans un temps raisonnable, nous allons travailler sur jeu de données réduit, composé de 6 fichiers .fastq.gz uniquement, correspondant aux 3 réplicats des conditions S2 (échantillons 3, 4 et 5) et S6 (échantillons 6, 7 et 8). Le jeu de données réduit réprésente 2,8 Go de données.

# Téléchargement du jeu de données

Sous Windows, ouvrez un terminal Ubuntu. Si vous avez oublié comment faire, consultez le [tutoriel Unix](https://omics-school.github.io/unix-tutorial/tutoriel/README).

Déplacez vous ensuite dans le répertoire `/mnt/c/Users/omics` :

```
$ cd /mnt/c/Users/omics
```

🔔 Rappel : Ne tapez pas le `$` en début de ligne et faites attention aux majuscules et au minuscules (surtout pour `Users`) !

Téléchargez le jeu de données de réduit qui se trouve sur le site Zenodo avec la commande `wget` :

```
$ wget 
```

Le fichier `rnaseq_sample.tgz` est une archive compressée (un peu comme un fichier zip). Décompressez-le avec la commande :
```
$ tar zxvf rnaseq_sample.tgz
```

Vérifiez que le répertoire `rnaseq_sample` est bien créé avec la commande `ls`.

# Exploration rapide des fichiers

Déplacez-vous tout d'abord dans le répertoire `rnaseq_sample` précédemment créé :
```
$ cd rnaseq_sample
```

Si vous avez bien effectué toutes les étapes précendes, la commande `pwd` doit vous renvoyer `/mnt/c/Users/omics/rnaseq_sample`. Vérifiez-le.

Affichez le contenu du répertoire courant, vous devriez obtenir quelque chose de ce type :

```
$ ls -l
total 0
drwxrwxrwx 1 duo duo 512 Jan 13 10:16 genome
-rwxrwxrwx 1 duo duo 500 Jan 13 10:17 md5sum.txt
drwxrwxrwx 1 duo duo 512 Jan 13 10:15 reads
```

Le répertoire `genome` contient le génome de *O. tauri* et ses annotations au format gff. Remarque : le génome de référence de *Ostreococcus tauri* et ses annotations sont disponibles sur la [page dédiée](https://www.ncbi.nlm.nih.gov/genome/373?genome_assembly_id=352933) sur le site du NCBI :
- [génome de référence](ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/214/015/GCF_000214015.3_version_140606/GCF_000214015.3_version_140606_genomic.fna.gz)
- [annotations](ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/214/015/GCF_000214015.3_version_140606/GCF_000214015.3_version_140606_genomic.gff.gz). Nous avons légèrement modifié le fichier d'annotations pour ne prendre en compte que les gènes et alléger la visualisation dans IGV.

Le répertoire `reads` contient les 6 fichiers *.fastq.gz*.

Une manière pratique de voir cette organisation est d'utiliser la commande `tree`

```
$ tree
.
├── genome
│   ├── GCF_000214015.3_version_140606.fna
│   └── GCF_000214015.3_version_140606.gff
├── md5sum.txt
└── reads
    ├── HCA-3_R1.fastq.gz
    ├── HCA-4_R1.fastq.gz
    ├── HCA-5_R1.fastq.gz
    ├── HCA-6_R1.fastq.gz
    ├── HCA-7_R1.fastq.gz
    └── HCA-8_R1.fastq.gz

2 directories, 9 files
```

Enfin, déterminez la taille de vos données avec la commande 
```
$ du -ch *
14M     genome
0       md5sum.txt
2.8G    reads
2.8G    total
```

Explications : la commande `du` affiche la taille occupée par des fichiers. L'option `-h` affiche la taille en ko, Mo, Go... L'option `-c` calcule la taille totale occupée par tous les fichiers.


# Vérification de l'intégrité des données

Nous avons téléchargé des données mais nous ne savons pas si le téléchargement s'est bien passé. Nous allons pour cela contrôler l'intégrité des données avec la commande `md5sum`.

Le fichier `md5sum.txt` contient les sommes de contrôle de tous les fichiers présents. Affichez son contenu avec la commande `cat` :

```
$ cat md5sum.txt
bfef14688f9cbcca45d794ec0348aa2e  genome/GCF_000214015.3_version_140606.gff
046b6e933274c884428a7d5929090f5d  genome/GCF_000214015.3_version_140606.fna
0f3cffa726234bdaf0cd2f62b8a45ffd  reads/HCA-3_R1.fastq.gz
2529abcf9ae6519c76c0b6b7f3e27f54  reads/HCA-4_R1.fastq.gz
0f898450100431936267bbf514055b9a  reads/HCA-5_R1.fastq.gz
c100b218bf6ee955d058561e8f53b881  reads/HCA-6_R1.fastq.gz
9e994b6120c74945177146732f8104f8  reads/HCA-7_R1.fastq.gz
854c101c83a38d25131de2f1ced5091d  reads/HCA-8_R1.fastq.gz
```

La colonne de gauche contient les sommes de contrôle MD5 et celle de droite les noms des fichiers correspondants.

On peut ainsi vérifier l'intégrité du fichier `GCF_000214015.3_version_140606.fna` situé dans le répertoire `genome` :
```
$ md5sum genome/GCF_000214015.3_version_140606.fna
046b6e933274c884428a7d5929090f5d  genome/GCF_000214015.3_version_140606.fna
```

Par comparaison avec le contenu du fichier `md5sum.txt`, on peut ainsi conclure que l'intégrité du fichier `genome/GCF_000214015.3_version_140606.fna` est bien vérifiée. Nous avons donc téléchargé le bon fichier :tada:

On peut alors faire la même chose pour tous les fichiers, mais ce serait fastidieux de le faire individuellement. Il est alors possible d'automatiser cette vérification avec l'option `-c` de la commande `md5sum` :
```
$ md5sum -c md5sum.txt
genome/GCF_000214015.3_version_140606.gff: OK
genome/GCF_000214015.3_version_140606.fna: OK
reads/HCA-3_R1.fastq.gz: OK
reads/HCA-4_R1.fastq.gz: OK
reads/HCA-5_R1.fastq.gz: OK
reads/HCA-6_R1.fastq.gz: OK
reads/HCA-7_R1.fastq.gz: OK
reads/HCA-8_R1.fastq.gz: OK
```

Si vous avez des *OK* partout, bravo ! Tous les fichiers sont corrects.

Vous pouvez passer à l'étape suivante.