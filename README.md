//Projeto Orientado a Objetos
//Ele é a ponte entre o código e o sistema de arquivos:
		
    import java.io.File

//Classe com parâmetro privado, imutável e que precisa de um arquivo:
		
    class FrasesA(private val Arquivo: String) {

//Função sem parâmetros que recebe uma String:
			
         fun FraseAleatoria(): String {

      //Cria um objeto que representa o arquivo cujo nome foi passado na variável Arquivo:
        val arquivo = File(Arquivo)

        //Verifica se o arquivo existe (encerra a função caso não exista e exibe uma mensagem):
          if (!arquivo.exists()) {
             return "Arquivo frases.txt não encontrado"
        }

        //Lê todas as linhas do arquivo e transforma em uma lista:
          val frases = arquivo.readLines()

        //Verifica se a lista está vazia ou não:
          return if (frases.isNotEmpty()) {

            //Escolhe uma frase aleatória da lista:
              frases.random()

        //Se o arquivo estiver vazio, retorna mensagem padrão:
          } else {
              "Sem frases cadastradas"
         }
       }
     }

//Classe com parâmetro privado, mutável e imutável:
		
    class TemporizadorPomodoro(
        private var tempoFoco: Int,
        private var tempoDescanso: Int,
        private val Frases: FrasesA
    ) {

        private var Foco = true

        fun iniciar() {
             while (true) {
                  if (Foco) {
                      iniciarFoco()
                  } else {
                      iniciarDescanso()
                  }

            //Inverte o valor:
                  Foco = !Foco
                }
              }

        private fun iniciarFoco() {
        println("\nFoco iniciado!")
        contarTempo(tempoFoco, "Foco")
    }

    private fun iniciarDescanso() {

        //Pega uma frase motivacional do arquivo:
        val frase = Frases.FraseAleatoria()

        println("\nHora do descanso!")
        contarTempo(
            tempoDescanso,
            "Descanso",
            frase
        )
    }

//Temporizador:

    private fun contarTempo(tempo: Int, modo: String, frase: String? = null) {

        //Começa um loop decrescente:
        for (i in tempo downTo 1) {

            //Conversão:
            val min = i / 60
            val seg = i % 60

            //Se a frase for diferente de nulo:
            if (frase != null) {

                //Exibe a frase:
                print("\r$modo: %02d:%02d | $frase".format(min, seg))
            } else {

                //Exibe só o tempo:
                print("\r$modo: %02d:%02d".format(min, seg))
            }

            //Pausa o código por um segundo:
            Thread.sleep(1000)
        }
        println()
     }
    }

//Inicia o programa por aqui:

    fun main() {

    //Repõe as frases:
       val trocar = FrasesA("frases.txt")

    //Objeto principal do sistema:
       val pomodoro = TemporizadorPomodoro(
          tempoFoco = 25 * 60,
          tempoDescanso = 5 * 60,
          Frases = trocar
       )

    //Inicia o prorama:
       pomodoro.iniciar()
    }
