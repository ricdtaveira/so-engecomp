Questionário — Processos e Threads

Capítulo 2 de "Sistemas Operacionais Modernos", de Andrew S. Tanenbaum e Herbert Bos.

Este questionário contém 20 questões, divididas em duas partes: Parte A (10 afirmações Verdadeiro/Falso) e Parte B (10 questões abertas). O gabarito está ao final.

Parte A — Verdadeiro ou Falso (10 questões)
( V ) ( F ) O modelo de processos permite que cada programa tenha a ilusão de possuir sua própria CPU, mesmo que a CPU física alterne rapidamente entre vários processos — fenômeno que Tanenbaum chama de pseudoparalelismo.
( V ) ( F ) Em sistemas UNIX, a chamada de sistema fork() substitui integralmente a imagem de memória do processo chamador por um novo programa.
( V ) ( F ) Uma das transições possíveis entre estados de um processo ocorre quando um processo em execução realiza uma chamada de sistema bloqueante e passa para o estado bloqueado.
( V ) ( F ) As threads de um mesmo processo não compartilham o espaço de endereçamento; cada uma possui sua própria cópia de código e dados.
( V ) ( F ) Na implementação de threads inteiramente no espaço do usuário, a troca entre threads é muito rápida, mas uma chamada de sistema bloqueante acaba travando todo o processo.
( V ) ( F ) A solução de Peterson para exclusão mútua depende de uma instrução especial de hardware, como a TSL (Test and Set Lock).
( V ) ( F ) Os semáforos, propostos por Dijkstra, possuem duas operações atômicas, tradicionalmente chamadas down (ou wait) e up (ou signal).
( V ) ( F ) O problema do jantar dos filósofos foi formulado para ilustrar como maximizar o throughput em sistemas de escalonamento em lote.
( V ) ( F ) O algoritmo Round-Robin é preemptivo, pois pode retirar a CPU de um processo ao final do seu quantum de tempo.
( V ) ( F ) No escalonamento por loteria (lottery scheduling), o processo com maior prioridade sempre vence o sorteio, garantindo um resultado determinístico.
Parte B — Perguntas Abertas (10 questões)
Explique, com base em Tanenbaum, a diferença entre "programa" e "processo".
Cite e descreva brevemente os quatro eventos que podem levar à criação de um processo.
Descreva os três estados básicos de um processo (execução, pronto e bloqueado) e explique duas das quatro transições possíveis entre eles.
O que é o bloco de controle de processo (PCB) e qual sua função dentro da tabela de processos mantida pelo sistema operacional?
Cite três dos quatro motivos apresentados por Tanenbaum para justificar o uso de threads em vez de apenas processos.
Compare a implementação de threads no espaço do usuário com a implementação no núcleo, apontando uma vantagem e uma desvantagem de cada abordagem.
O que é uma condição de corrida (race condition)? Descreva um exemplo.
Liste as quatro condições que, segundo Tanenbaum, uma boa solução de exclusão mútua deve satisfazer.
Explique o funcionamento de um semáforo e indique a principal diferença entre um semáforo e um mutex.
Explique os objetivos do escalonamento em sistemas em lote e em sistemas interativos, apontando uma diferença entre eles.
Gabarito
Parte A — Verdadeiro ou Falso
Verdadeiro. É exatamente a definição de pseudoparalelismo dada por Tanenbaum: o SO cria a ilusão de CPUs virtuais dedicadas, ainda que a CPU real seja compartilhada por alternância rápida.
Falso. Quem cria uma cópia do processo pai é a própria chamada fork(). A substituição da imagem de memória por um novo programa é feita por uma chamada subsequente, como exec().
Verdadeiro. Essa é uma das quatro transições descritas por Tanenbaum: execução → bloqueado, quando o processo aguarda um evento externo (como o término de uma operação de E/S).
Falso. As threads de um mesmo processo compartilham o espaço de endereçamento (código, dados, arquivos abertos); cada uma mantém apenas seu próprio contador de programa, registradores e pilha.
Verdadeiro. É justamente a limitação apontada por Tanenbaum para essa abordagem: o núcleo não sabe que existem múltiplas threads, então uma chamada bloqueante bloqueia o processo inteiro.
Falso. A solução de Peterson é uma solução puramente de software, baseada em uma variável de "vez" e um vetor de intenção — não exige nenhuma instrução especial de hardware.
Verdadeiro. Semáforos são variáveis inteiras com duas operações atômicas — down/wait (decrementa, bloqueando se necessário) e up/signal (incrementa, podendo acordar um processo em espera).
Falso. O jantar dos filósofos ilustra deadlock e inanição (starvation) na disputa concorrente por múltiplos recursos compartilhados, não questões de throughput de sistemas em lote.
Verdadeiro. Ao final do quantum, o SO pode preemptivamente retirar a CPU do processo em execução e recolocá-lo no fim da fila de prontos, dando a vez ao próximo processo.
Falso. O escalonamento por loteria é probabilístico: mais bilhetes aumentam a chance de um processo ser sorteado, mas não garantem a vitória — não há determinismo.
Parte B — Perguntas Abertas
Programa é uma entidade passiva — o código armazenado, como uma "receita". Processo é uma entidade ativa: uma instância de um programa em execução, com seus próprios valores de contador de programa, registradores e variáveis (o "cozinheiro" executando a receita).
(1) Inicialização do sistema; (2) execução de uma chamada de sistema de criação de processo por um processo já em execução (ex.: fork); (3) requisição do usuário para criar um novo processo; (4) início de uma tarefa em lote (batch).
Em execução (running): o processo está usando a CPU naquele instante. Pronto (ready): o processo é executável, mas aguarda sua vez de usar a CPU. Bloqueado (blocked): o processo não pode ser executado até que ocorra algum evento externo. Duas transições possíveis (aceitar quaisquer duas): execução → bloqueado (o processo espera por E/S ou faz uma chamada de sistema bloqueante); execução → pronto (o escalonador retira a CPU, geralmente ao fim do quantum); pronto → execução (o escalonador escolhe esse processo); bloqueado → pronto (o evento esperado ocorre).
O PCB é a entrada da tabela de processos correspondente a um processo específico. Ele armazena as informações necessárias para gerenciar o processo — registradores, contador de programa, ponteiro de pilha, estado do processo, além de dados de gerenciamento de memória e de arquivos — permitindo ao SO salvar e restaurar o contexto do processo para retomá-lo exatamente de onde parou.
Qualquer três destes quatro: permitir paralelismo dentro de uma mesma aplicação; serem mais fáceis e mais rápidas de criar e destruir que processos (não exigem duplicar o espaço de endereçamento); não prejudicarem o desempenho quando há computação e E/S intercaladas (uma thread continua processando enquanto outra aguarda E/S); explorarem melhor sistemas com múltiplos núcleos (multiprocessadores reais).
Threads no espaço do usuário — vantagem: troca de contexto entre threads muito rápida, sem exigir chamada de sistema; desvantagem: uma chamada de sistema bloqueante trava todo o processo, pois o núcleo não enxerga as threads individualmente. Threads no núcleo — vantagem: uma thread bloqueada não trava as demais threads do mesmo processo; desvantagem: cada operação sobre uma thread exige uma chamada de sistema, o que é mais custoso.
Uma condição de corrida ocorre quando o resultado final de uma execução concorrente depende da ordem exata em que as operações de diferentes processos ou threads acontecem. Exemplo clássico de Tanenbaum: o spooler de impressão, em que dois processos leem a mesma variável indicando o próximo slot livre da fila e ambos tentam gravar seus arquivos nesse mesmo slot — fazendo um dos arquivos se perder, dependendo de qual processo "vence" a corrida.
(1) Dois processos jamais podem estar simultaneamente dentro de suas regiões críticas; (2) nenhuma suposição pode ser feita sobre velocidades relativas dos processos ou sobre o número de CPUs; (3) nenhum processo executando fora de sua região crítica pode bloquear outros processos; (4) nenhum processo deve ter que esperar eternamente para entrar em sua região crítica.
Um semáforo é uma variável inteira manipulada apenas por duas operações atômicas: down (ou wait), que decrementa o valor e bloqueia o processo se o resultado for negativo, e up (ou signal), que incrementa o valor e pode acordar um processo em espera. Semáforos podem ser usados tanto para exclusão mútua quanto para sincronizar a ordem de eventos entre processos. Já o mutex é uma versão simplificada do semáforo, com apenas dois estados (livre/ocupado), voltada especificamente para exclusão mútua entre threads.
Em sistemas em lote, os objetivos centrais são maximizar o throughput (número de tarefas concluídas por hora) e a utilização da CPU, além de minimizar o turnaround time — sem se preocupar com a espera percebida por um usuário interativo. Em sistemas interativos, o objetivo central passa a ser minimizar o tempo de resposta e atender à proporcionalidade esperada pelo usuário. A diferença fundamental é que sistemas em lote priorizam a vazão total do sistema, enquanto sistemas interativos priorizam a experiência percebida por quem está esperando uma resposta em tempo real.