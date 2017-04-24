# Resumo

Cap. 1
  É preciso fazer um request via código para acessar determinada url. Para tanto, é preciso usar a seguinte estrutura:
    from urllib.request import urlopen
    html = urlopen("url.html")
    
    # urllib é uma das bibliotecas mais utilizadas para scrapping: https://docs.python.org/3/library/urllib.html
    ## urlopen abre um objeto remoto em uma rede e o armazena
  
  BeautiSoup:
    Biblioteca que ajuda a formatar e organizar dados não estruturados organizados de maneira confusa ou mal-escritos através da correção     de "bad HTML files", nos apresentando objetos facilmente legíveis em Python que representam estrutura XML.
