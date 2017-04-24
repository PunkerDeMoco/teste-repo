# Resumo
________________________________________________________________________________________________________________________________________
Cap. 1 - Your First Web Scraper
  * É preciso fazer um request via código para acessar determinada url. Para tanto, é preciso usar a seguinte estrutura:
    from urllib.request import urlopen
    html = urlopen("url.html")
    
   # Output: b'<html>\n<head>\n<title>A Useful Page</title>\n</head>\n<body>\n<h1>An Interesting Title</h1>\n<div>\nLorem ipsum dolor      sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in            voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui          officia deserunt mollit anim id est laborum.\n</div>\n</body>\n</html>\n'
    
   # urllib é uma das bibliotecas mais utilizadas para scrapping: https://docs.python.org/3/library/urllib.html
   ## urlopen abre um objeto remoto em uma rede e o armazena
_______________________________________________________________________________________________________________________________________ 
 * BeautiSoup:
  Biblioteca que ajuda a formatar e organizar dados não estruturados organizados de maneira confusa ou mal-escritos através da               correção     de "bad HTML files", nos apresentando objetos facilmente legíveis em Python que representam estrutura XML.
    
   # https://www.crummy.com/software/BeautifulSoup/bs4/doc/
   ## !pip install beautifulsoup4 - instala o pacote através do Jupyter Notebook
      
   O comando mais utilizado na biblioteca deriva da criação de um objeto bs4. Podemos abrir uma página HTML utilizando a biblioteca:
      
   from bs4 import BeautifulSoup as bs
   from urllib.request import urlopen
   html = urlopen("http://pythonscraping.com/pages/page1.html")
     bsObj = BeautifulSoup(html.read())
     print(bsObj)

   # Output: 
   <html>
   <head>
   <title>A Useful Page</title>
   </head>
   <body>
   <h1>An Interesting Title</h1>
   <div>
   Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim     ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in             reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt       in   culpa qui officia deserunt mollit anim id est laborum.
   </div>
   </body>
   </html>

   Percebe-se a diferença de estrutura entre os dois outputs. Ainda, pelo objeto bs, podemos trazer como output apenas uma parte            específica da página. Ex:o código print(bsObj.h1) retornaria apenas a linha <h1>An Interesting Title</h1> como resultado.

   Ainda, é possível criar ambientes virtuais através do Python, o que é importante para separar as bibliotecas utilizadas para cada        projeto, facilitando a administração dos mesmo (avitando, inclusive, conflitos entre versões diferentes das bibliotecas).
________________________________________________________________________________________________________________________________________
  * Controlando erros e exceções. Quando trabalhamos com scrapping, é possível que muitas páginas retornem erros de conexão ou de servidor   e é preciso controlar tais possibilidade. O código abaixo avalia se:
  
   I) A página é encontrada no server (erro HTTP). Se sim, retorna o cógido normalmente, se não, retorna o erro printado na tela  
   II) Caso o servidor não seja encontrado, a função urlopen retorna um objeto tipo None.  

   Podemos aplicar a mesma lógica para testar tags específicas de um objeto BS. Se a biblioteca tentar acessar uma tag inexistente na      página html ou XML, a mesma retornará um objeto do tipo None. Se tentarmos aplicar funções ou métodos à este objeto None,                receberemos   um "Attribute Error". Reescrevendo o exemplo acima, controlando pelos quatro erros comentados:
   
           from urllib.request import urlopen
           from urllib.error import HTTPError
           from bs4 import BeautifulSoup
           def getTitle(url):
             try:
               html = urlopen(url)
             except HTTPError as e:
               return None
             try:
               bsObj = BeautifulSoup(html.read())
               title = bsObj.body.h1
             except AttributeError as e:
               return None
             return title
           title = getTitle("http://www.pythonscraping.com/exercises/exercise1.html")
           if title == None:
             print("Title could not be found")
           else:
             print(title)
_______________________________________________________________________________________________________________________________________ 
Cap. 2 - Advanced HTML Parsing
 * BeautifulSoup pode pesquisar tags específicas por seus atributos, trabalhar com listas de tags e analisar árvores de navegação 

     html = urlopen("http://www.pythonscraping.com/pages/warandpeace.html")
     bsObj = BeautifulSoup(html)
     names_list = bsObj.findAll("span", {"class":"green"}) #função .findALL(tagName, tagAttributes) extrai para uma lista todos os            objetos que satisfazem os argumentos  
     for name in names_list:
        print(name)
   
     # Output: 
     <span class="green">Anna Pavlovna Scherer</span>
     <span class="green">Empress Marya Fedorovna</span>
     <span class="green">Prince Vasili Kuragin</span>
     <span class="green">Anna Pavlovna</span>
   
     Podemos printar apenas o texto de cada objeto separadamente de suas tags:
     for name in names_list:
        print(name.get_text())
        
     # Output: 
     Anna
     Pavlovna Scherer
     Empress Marya
     Fedorovna
     Prince Vasili Kuragin
     Anna Pavlovna
_______________________________________________________________________________________________________________________________________ 
 * Other BeautifulSoup Objects
  Além dos objetos estudados (bsObj e tag Objects), a biblioteca possui mais dois tipos de objetos:
   NavigableString Object: Usado para representar texto em tags, em vez das próprias tags
   The Comment Object: Usado para encontrar comentários HTML em tags de comentário, <!--como este-->
_______________________________________________________________________________________________________________________________________ 
 * Árvores de Navegação
  São úteis quando precisamos encontrar tags com base em suas localizações em um documento.
  
  next_siblings e previous_siblings - funções adequeadas para extrair dados de tabelas, especialmente aquelas com linhas de título.
     from urllib.request import urlopen
     from bs4 import BeautifulSoup
     html = urlopen("http://www.pythonscraping.com/pages/page3.html")
     bsObj = BeautifulSoup(html)
     for sibling in bsObj.find("table",{"id":"giftList"}).tr.next_siblings:
       print(sibling)
       
  # O output do código imprimi todas as linhas de produtos da tabela de produtos, exceto para a primeira linha do título
_______________________________________________________________________________________________________________________________________ 
 * Expressões Regulares
  A depender do caso, boa maneira de tratar textos regulares dentro de um documento (como possíveis endereços de email ou telefones)
  Principais regras para expressões regulares: pgs. 50 - 52
   Expressão regular para identificação de emails: [A-Za-z0-9\._+]+@[A-Za-z]+\.(com|org|edu|net)
   
  Também é possível usar as expressões regulares para buscar de maneira eficiente apenas as imagens que correspondem à cesta de produtos   do exemplo anterior:
  images = bsObj.findAll("img", {"src":re.compile("\.\.\/img\/gifts/img.*\.jpg")})
  for image in images:
    print(image["src"])
    
  # Output: 
  ../img/gifts/img1.jpg
  ../img/gifts/img2.jpg
  ../img/gifts/img3.jpg
  ../img/gifts/img4.jpg
  ../img/gifts/img6.jpg
_______________________________________________________________________________________________________________________________________
 * Funções Lambda
  Nos permite passar uma função como variável de outra função. f(g(x), y), f(g(x), h(x)).
  BeautifulSoup nos permite passar certos tipos de funções como parâmetros para a função findAll. A única restrição é que essas funções   devem ter um objeto tag como um argumento e retornar um valor boolean como output.
  
 * Outras bibiliotecas úteis
  Além de BeautifulSoup, o Python possui outras bibliotecas para análise de arquivos HTML:
  lmxl: analisa documentos HTML e XML e é conhecido por ser fortemente baseado em C e por sua rapidez de análise.
  HTML Parser: biblioteca de análise integrada do Python. Como não requer instalação, pode ser extremamente conveniente.
_______________________________________________________________________________________________________________________________________ 
Cap. 3 - Starting to Crawl
 
