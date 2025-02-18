# Caminhar

O processo de caminhar do robô é dividido em 3 etapas:  

1. O [step planner](./step-planning.md) calcula os passos que o robô deve dar. Isso inclui coordenadas x, y, z e rotação dependendo do `motion_command`.  
2. O **walk manager** usa o passo calculado anteriormente e o `motion_command` para criar o `walk_command`, que define o modo de caminhar, como ficar em pé, andar para frente, para trás, etc.  
3. A **walking engine** recebe o `walk_command` e o passo calculado e computa de acordo com os comandos de motores.  

## Walking Engine

O núcleo da Engine de Caminhada é baseado no trabalho de [Bernhard Hengst](https://www.researchgate.net/profile/Bernhard-Hengst) da [UNSW Sydney](https://www.unsw.edu.au/), que é padrão na liga.  
A idea por trás é bem simples:

1. Há dois pés, o *pé de suporte* e o *pé de balanço*.  
2. Mova o *pé de balanço* para frente na velocidade *x²*.  
3. Mova o *pé de suporte* para trás na velocidade *x*.

Detalhes técnicos podem ser encontrados na página do [Step Planning](./step-planning.md).
