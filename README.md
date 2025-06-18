# RTOS: Semáforos e Mutex com FreeRTOS (CMSIS-RTOS v2)

[cite_start]Este repositório contém as soluções para os exercícios de "Semáforos com FreeRTOS (CMSIS-RTOS v2)".  O objetivo é demonstrar o uso de mecanismos de sincronização, como semáforos (contadores e binários) e mutex, para gerenciar o compartilhamento de recursos e a coordenação entre tarefas em um sistema de tempo real.

Os códigos foram desenvolvidos para a plataforma STM32, utilizando o STM32CubeIDE e a camada de abstração CMSIS-RTOS v2 sobre o FreeRTOS.

## Exercícios Implementados

Aqui está a descrição de cada atividade presente no repositório.

### Exercício 1: Observação de Comportamento com Semáforo Contador
* **Objetivo:** Avaliar o funcionamento de tarefas que compartilham um semáforo contador. 
* **Atividades:**
    * Criação de três tarefas que utilizam `osSemaphoreAcquire()` para obter um de dois recursos disponíveis. 
    * Uso de `osSemaphoreGetCount()` para fins de depuração. 
    * Análise do que ocorre quando uma tarefa tenta adquirir um semáforo cuja contagem já está em zero. 
* **Pasta:** `Atividade_RTOS-4.1`

### Exercício 2: Liberação de Semáforo via Interrupção
* **Objetivo:** Sincronizar tarefas com a liberação do semáforo por uma interrupção externa (botão de usuário). 
* **Atividades:**
    * Criação de um semáforo com valor inicial igual a zero. 
    * As tarefas aguardam a liberação do semáforo com `osSemaphoreAcquire()`. 
    * A liberação é feita com `osSemaphoreRelease()` dentro da rotina de callback `HAL_GPIO_EXTI_Callback()`. 
    * A execução das tarefas ocorre de forma sequencial, conforme o botão é pressionado. 
* **Pasta:** `Atividade_RTOS-4.2`

### Exercício 3: Teste de Disponibilidade e Liberação Manual
* **Objetivo:** Demonstrar que um semáforo contador não garante a propriedade do recurso, ao contrário de um mutex. 
* **Atividades:**
    * A tarefa verifica a contagem do semáforo com `osSemaphoreGetCount()`. 
    * Se a contagem for zero, a própria tarefa o libera (`release()`) e em seguida o adquire (`acquire()`), mostrando que pode acessar o recurso sem ser "dona" dele. 
* **Pasta:** `Atividade_RTOS-4.3`

### Exercício 4: Tentativa Indevida de Liberação de Mutex
* **Objetivo:** Mostrar que apenas a tarefa que adquire um mutex pode liberá-lo. 
* **Atividades:**
    * A `Task1` adquire um mutex e não o libera. 
    * A `Task2` tenta liberar o mesmo mutex. 
    * O status de erro `osErrorResource` é verificado para confirmar que a operação falhou. 
* **Pasta:** `Atividade_RTOS-4.4`

### Exercício 5: Coleta, Processamento e Reinicialização com Interrupção Externa
* **Objetivo:** Usar dois semáforos para coordenar uma cadeia de tarefas (coleta ADC, cálculo de média e envio UART), com um mecanismo de reinício via botão. 
* **Atividades:**
    * `TaskADC` coleta 100 amostras e libera um semáforo para a `TaskCalc`. 
    * `TaskCalc` calcula a média e libera um semáforo para a `TaskSend`. 
    * `TaskSend` envia a média via UART. 
    * O processo se repete 10 vezes e, ao final, aguarda o botão do usuário para reiniciar. 
* **Pasta:** `Atividade_RTOS-4.5`

### Exercício 6: Sincronização Completa por Evento Externo
* **Objetivo:** Implementar uma cadeia de tarefas completa (aquisição, cálculo, armazenamento e transmissão), iniciada e sincronizada por um único evento de botão. 
* **Fluxo de Execução:**
    1.  **Botão Pressionado:** Uma interrupção (`HAL_GPIO_EXTI_Callback`) libera o semáforo `semStartHandle` para iniciar a cadeia. 
    2.  **TaskADC:** Aguarda `semStartHandle`, realiza 100 leituras do ADC, armazena em um buffer e libera `semCalcHandle`. 
    3.  **TaskCalc:** Aguarda `semCalcHandle`, calcula a média dos valores e libera `semSaveHandle`. 
    4.  **TaskSave:** Aguarda `semSaveHandle`, salva a média em um vetor de histórico e libera `semSendHandle`. 
    5.  **TaskSend:** Aguarda `semSendHandle` e envia a média calculada pela UART. 
* **Pasta:** `Atividade_RTOS-4.6.1`

---
*Este repositório e seus conteúdos foram desenvolvidos como parte das atividades da disciplina de Sistemas de Tempo Real. Conforme solicitado, os códigos produzidos foram inseridos neste projeto do GitHub.*
