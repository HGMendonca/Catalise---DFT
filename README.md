# Catalise---DFT
Mini tutorial sobre como fazer cálculos DFT (VASP) a fim de obter as barreiras das Energias Livres de Gibbs (ΔG) para processos de eletrocatálise.


Vamos considerar nesse tutorial que temos uma estrutura de grafeno modificada por um metal de transição (TM) coordenado por quatro Nitrogênios.
Nosso objetivo é calcular a barreira ΔG quando um único átomo de Hidrogênio (H) é adsovido no TM. Para tanto, devemos seguir os procedimentos:

1. Relaxar a estrutura grafeno @ TM N_4.
   - Preferencialmente usando ISIF = 3, ISPIN = 2, e correção de U (DFT+U).
2. Calcular a estrutura de bandas
   - Aqui é necessário verificar se as bandas do material (por exemplo grafeno @ TM N_4) são metálicas. Pois a eletrocatálise só pode ocorre      com alta eficiência se o substrato for um fornecedos de elétrons.
3. Relaxar a estrutura grafeno @ TM N_4 com átomo de H adsorvido ao TM.
   - Aqui não é necessário fazer a relaxação usando ISIF = 3, pode ser usado ISIF = 2.
   - Para algumas reações química, para descrever as barreiras corretamente é necessário incluir a flag MAGMOM, a fim de capiturar estados        singleto e/ou tripletos das espécies adsorvidas.
4. Cálculo da Energia de Ponto-Zero (ZPE) e da correção de Entropia.
   - Para essa etapa devos fixar grafeno @ TM N_4, e deixar o H livre para movimentar, pois essencialmente estamos fazendo um cálculo das
     frequências de vibração.
   - FLAGS fundamentais:
       -- NSW    =  1            (number of ionic steps. Make it odd.)
       -- IBRION =  5            (frequence calculation)
       -- POTIM  =  0.02         (displacement step)
       -- NFREE  =  2            (displacement freedom)
5. Usando VASPKIT: Opção 501, e insira a Temperatura desejada.

Após esses passos já estamos prontos para calcular ΔG.
É necessário ter: 
- Energia total do sistema sem H -> E*
- Energia total do sistema com H -> E^H
- Energia de Ponto-Zero -> ZPE
- Entropia -> S
- Temperatura -> T

OBS. É necessário também que tenha congervido a energia total, o ZPE e S da molécula de H_2.

Com todos esses ingredientes basta fazer o cálculo:

             ΔG_{H^*} = E^{H^*} + ZPE^{H^*} - TS^{H^*} - E^* - 0.5 (E^{H_2} + ZPE^{H_2} - TS^{H_2})
             
