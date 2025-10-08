# Aulao_Git

## Metodologia (parte 1): obtenção dos organismos e de seus KOs associados

1- Download dos organismos no caminho /home/alinereis/mestrado/data/

`wget http://rest.kegg.jp/list/organism`


2- Subdivisão dos eucariotos em Fungos e não-fungos

`grep 'Fungi;' organism > Fungi_info_list.txt` 

`grep 'Eukaryotes;' organism | grep -v 'Fungi;' > Eukanofungi_info_list.txt`


3- Obtenção da lista de IDs de cada grupo 

`cat Eukanofungi_info_list.txt | awk '{print$1}' > ID_Eukanofungi.txt` 

`cat Fungi_info_list.txt | awk '{print$1}' > ID_Fungi.txt`


4- Associação dos KOs aos organismos de cada grupo

`for i in ´less ID_Fungi.txt´; do echo wget http://rest.kegg.jp/link/ko/$i ; done > download_Fungi.sh`

`for i in ´less ID_Eukanofungi.txt´; do echo wget http://rest.kegg.jp/link/ko/$i ; done > download_Eukanofungi.sh`


5- Cada arquivo foi direcionado e executado nos respectivos diretórios: 

*Fungi_ID_KO e Eukanofungi_ID_KO*


6- Curadoria dos arquivos para rodar no KOMODO2

`for i in T*; do sed -i -e 's/ko://g' ${i} ;done`

`for i in ´ls T*´; do cat header  $i > $i.comheader ; done`


7- Extração de todos os KOs dos 2 grupos de organismos e concatenação em um só arquivo sem repetições:

`cat * | awk '{print $2}' | sort | uniq > koEukanofungi`  

`cat * | awk '{print $2}' | sort | uniq > koFungi`

`cat koEukanofungi koFungi | sort | uniq > kosemdescricao.txt`


8- Download do arquivo de todos os KOs do KEGG com sua descrição

`wget http://rest.kegg.jp/list/ko`
 

9- Associação da lista dos IDs dos KOs às suas descrições

`grep -f ko_sem_descricao.txt ko_organisms > dicionário_eukaryotes`

