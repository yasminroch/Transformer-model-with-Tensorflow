## Transformers com Tensorflow

No modelo construído foi feitas três observações diferentes e rodagens para tirar conclusões tanto positivas quanto negativas.

Na primeira rodagem, o modelo foi rodado tendo como mudança o número de épocas durante o treinamento. De 20 épocas foi para 1 época e, mesmo com essa mudança, o modelo demorou quase quatro horas para rodar com o modelo A100 GPU. Desse modo, o tempo de treinamento foi um empecilho. Como tentativa de diminuir o tempo de treinamento para cada época, foi mudado os seguintes parâmetros:

Mudança no batch size: de 64 foi para 32
Mudança no d_model: de 512 foi para 256
Mudança no num_heads: de 8 foi para 2
Mudança no max_tokens: de 128 foi para 64
Mudança no train_batches: de 1 foi para 10 com a finalidade para fazer um teste rápido

Tais mudanças resultaram em um tempo de treinamento um pouco menor mesmo com steps maiores por época, mas ainda assim foi longo, sendo cerca de três horas com a A100 GPU.

Na última e terceira rodagem, foi adicionado .cache() na função def make_batches para evitar que etapas da pipeline sejam recomputadas a cada época de treinamento. No entanto, o tempo de treinamento permaneceu em três horas, devido ao tempo excessivo, não foi possível melhorar a acurácia do modelo.

## Pontos positivos
* Ao longo da observação, foi possível concluir que o Transformer é ótimo ao se tratar da paralelização. Diferente do modelo RNN por exemplo, onde não é paralelizado e sim sequencial, o Transformers permitiu um treinamento rápido (Se fosse um modelo sequencial, provavelmente demoraria mais do que a implementação do Transformer demorou), visto que as operações são paralelizadas durante o treinamento - [Fonte de consulta](https://medium.com/@mroko001/rnn-vs-lstm-vs-transformers-unraveling-the-secrets-of-sequential-data-processing-c4541c4b09f);
* Captura de contexto: o Transformer tem um mecanismo de autoateção que constribui para a captura de relações dentro de um texto, pois vai ponderando o nível de importância de cada parte do input em relação a outra parte.

## Pontos negativos
* Tempo de treinamento e acurácia: o treinamento do Transformer levou muito tempo, resultando em algumas vezes além do tempo excessivo, a interrupção da sessão. Por ter apenas uma época, contribuiu para que a acurácia não fosse boa suficiente, sendo apenas 1% de acurácia;
* Consumo de recursos e ambiente de execução: necessidade de rodar em um ambiente como A100 GPU, sem CPU ou T4 GPU por ter problemas de estourar a RAM, exigindo um poder computacional avançado.
