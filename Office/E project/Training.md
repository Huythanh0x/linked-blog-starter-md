
### Training table

|                |         | Not null | unique |                                                              |                                                                          |
| -------------- | ------- | -------- | ------ | ------------------------------------------------------------ | ------------------------------------------------------------------------ |
| trainingId     | varchar | Yes      | Yes    |                                                              |                                                                          |
| trainingType   | tinyInt | Yes      |        | 0: VOICE  <br>1: CHAT                                        |                                                                          |
| traningMode    | tinyInt | Yes      |        | 0: persuasive  <br>1: negociation                            |                                                                          |
| level          | int     | Yes      |        | 0: BASIC  <br>1: INTERMEDIATE  <br>2: ADVANCED  <br>3: DEMON |                                                                          |
| userId         | varchar | Yes      |        |                                                              | FK [user.id](http://user.id/)                                            |
| productId      | varchar |          |        | null                                                         | FK: training_product                                                     |
| background     | varchar |          |        | null                                                         | FK: [training_role_personality.id](http://training_role_personality.id/) |
| voicingModel   | tinyInt |          |        | 0                                                            | 0: no detecting  <br>1: Gemini  <br>2: eleventLab                        |


### Training details
|                      |           |          |        |                                                  |                                                 |
| -------------------- | --------- | -------- | ------ | ------------------------------------------------ | ----------------------------------------------- |
| Physical name        | Data type | Not Null | Unique | Default                                          | Extra                                           |
| trainingDetailId     | varchar   | Yes      | Yes    |                                                  |                                                 |
| trainingId           | varchar   | Yes      |        |                                                  | FK [Training_data.id](http://training_data.id/) |
| status               | tinyInt   | yes      |        | 0: In processing  <br>1: abort  <br>2: completed |                                                 |
| averagescore         | float     |          |        | 0                                                |                                                 |
| scoreQuestion        | float     |          |        | 0                                                |                                                 |
| scoreClosing         | float     |          |        | 0                                                |                                                 |
| scoreLogical         | float     |          |        | 0                                                |                                                 |
| scoreSuggestion      | float     |          |        | 0                                                |                                                 |
| scoreListening       | float     |          |        | 0                                                |                                                 |
| evaluationQuestion   | varchar   |          |        |                                                  |                                                 |
| evaluationClosing    | varchar   |          |        |                                                  |                                                 |
| evaluationLogical    | varchar   |          |        |                                                  |                                                 |
| evaluationSuggestion | varchar   |          |        |                                                  |                                                 |
| evaluationListening  | varchar   |          |        |                                                  |                                                 |

