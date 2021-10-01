![](https://github.com/ocpjp-study/modules-migration/blob/main/modules-migration.png)

### Tópico: Migrando para uma aplicação modular
### Objetivos
- Migrar uma aplicação que use JDK <= 8, para JDK 11
- Usar jdeps para determinar dependências e identificar maneiras de resolver o ciclo de dependências

<hr>

### Resumo
<br/>

> #### Módulo Nomeado
  - Contém um module-info.java;
  - Exporta os pacotes que forem informados no module-info.java;
  - É legível por outro módulo no module-path;
  - É legível por outro JAR no classpath;
<br/><br/>
  
> #### Módulo Automático  
  - Não contém um module-info.java;
  - Exporta todos os pacotes;
  - É legível por outro módulo no module-path;
  - É legível por outro JAR no classpath;
  - Se o MANIFEST.MF declarar o atributo Automatic-Module-Name, usa ele, senão:
    - Remover a extensão do arquivo “.jar”;
    - Remover qualquer informação referente a versão do “.jar”;
    - Renomear qualquer caractere remanescente que NÃO seja letra e número, por “.” (ponto);
    - Renomear para que fique apenas 1 ponto sozinho;
    - Remover “.” (ponto) se ele for o primeiro ou o último caractere do resultado;
<br/><br/>
  
> #### Módulo sem nome 
  - Ignora se conter um module-info.java;
  - Não exportam qualquer pacote;
  - Podemos considerar que é um “.jar” comum, antes de existirem a ideia de módulos;
  - Se contém um module-info.java, será ignorado;
  - NÃO é legível por outro módulo no module-path;
  - É legível por outro JAR no classpath;
<br/><br/>
  
> #### Migrando Bottom-UP
  - Adicione um module-info.java no projeto. Adicione exports para todos os pacotes que deseja expor. Também adicione requires para informar suas dependências;
  - Mover de classpath para module-path;
  - Garanta que todos os módulos que ainda não foram migrados, permaneçam sem nome no classpath;
  - Repita todos os passos com os outros projetos de menor nível até o fim;		
<br/><br/>

> #### Migrando Top-Down
  - Coloque todos os projetos dentro de um module-path;
  - Escolha o projeto do nível mais alto que ainda não foi migrado;
  - Adicione o arquivo module-info.java a esse projeto para converter o módulo automático em um módulo nomeado. Novamente, lembre-se de adicionar qualquer exportação ou diretivas necessárias. Você pode usar o nome do módulo automático de outros módulos ao escrever a diretiva requer, uma vez que a maioria dos projetos no caminho do módulo ainda não tem nomes;
  - Repita os passos até acabarem os projetos;
<br/><br/>
  
> #### Comparando Migrações
  - Um projeto que depende de todos os outros projetos:
    - Bottom-Up: sem nome, dentro do classpath;
    - Top-Down: nomeado, dentro do module-path;
  - Um projeto que não tem dependências:
    - Bottom-Up: nomeado, dentro do module-path;
    - Top-Down: automático, dentro do módulo path;
<br/><br/>

> #### Outros
  - Não é permitido ter dependência cíclica entre módulos.
  - jdeps `--jdk-internals` “zoo.dino.jar”: verifica dependências.
