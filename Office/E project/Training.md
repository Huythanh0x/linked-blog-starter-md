
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
| trainingStatus | tinyInt | Yes      |        | 0: IN_PROGRESS<br>1: FINISHED                                |                                                                          |

