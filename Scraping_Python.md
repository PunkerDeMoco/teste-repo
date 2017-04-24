# Resumo

Cap. 1
  É preciso fazer um request via código para acessar determinada url. Para tanto, é preciso usar a seguinte estrutura:
    from urllib.request import urlopen
    html = urlopen("url.html")
    
   ### Output: ''b'<html>\n<head>\n<title>A Useful Page</title>\n</head>\n<body>\n<h1>An Interesting Title</h1>\n<div>\nLorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui            officia deserunt mollit anim id est laborum.\n</div>\n</body>\n</html>\n''''
    
   # urllib é uma das bibliotecas mais utilizadas para scrapping: https://docs.python.org/3/library/urllib.html
   ## urlopen abre um objeto remoto em uma rede e o armazena
  
 BeautiSoup:
  Biblioteca que ajuda a formatar e organizar dados não estruturados organizados de maneira confusa ou mal-escritos através da               correção     de "bad HTML files", nos apresentando objetos facilmente legíveis em Python que representam estrutura XML.
    
    # https://www.crummy.com/software/BeautifulSoup/bs4/doc/
    ## !pip install beautifulsoup4 - instala o pacote através do Jupyter Notebook
      
  O comando mais utilizado na biblioteca deriva da criação de um objeto bs4. Podemos abrir uma página HTML utilizando a biblioteca:
      
  from bs4 import BeautifulSoup as bs
  from urllib.request import urlopen
  html = urlopen("http://pythonscraping.com/pages/page1.html")
    bsObj = BeautifulSoup(html.read())
    print(bsObj)
      
  ### Output: 
  ''<html>
  <head>
  <title>A Useful Page</title>
  </head>
  <body>
  <h1>An Interesting Title</h1>
  <div>
  Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim     ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in             reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in   culpa qui officia deserunt mollit anim id est laborum.
  </div>
  </body>
  </html>''
   
  Percebe-se a diferença de estrutura entre os dois outputs. Ainda, pelo objeto bs, podemos trazer como output apenas uma parte           específica da página. Ex:o código print(bsObj.h1) retornaria apenas a linha '<h1>An Interesting Title</h1>' como resultado.
  
  Ainda, é possível criar ambientes virtuais através do Python, o que é importante para separar as bibliotecas utilizadas para cada       projeto, facilitando a administração dos mesmo (avitando, inclusive, conflitos entre versões diferentes das bibliotecas).
    
    
