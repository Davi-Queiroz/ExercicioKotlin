Davi Queiroz Santos

RM 557483

1. Parâmetro obrigatório na tela de Perfil

Inicialmente, a tela de perfil utilizava uma rota fixa:

composable(route = "perfil")
navController.navigate("perfil")

Nesse cenário, a PerfilScreen não recebia nenhum dado externo e sempre exibia um conteúdo fixo:

text = "PERFIL"

Com a mudança para uma rota parametrizada:

composable(route = "perfil/{nome}")
navController.navigate("perfil/Fulano de Tal")

O valor passa a ser enviado na própria navegação e recuperado via argumentos:

val nome = it.arguments?.getString("nome")

A tela então passa a depender desse dado:

text = "PERFIL - $nome"

Essa abordagem funciona porque o placeholder {nome} permite mapear parte da URL para um argumento acessível dentro do composable.

2. Parâmetro opcional na tela de Pedidos

Na tela de pedidos, foi adotada uma abordagem mais flexível, com parâmetro opcional:

composable(
route = "pedidos?cliente={cliente}",
arguments = listOf(navArgument("cliente") {
defaultValue = "Cliente Genérico"
})
)

O valor pode ou não ser enviado:

val cliente = it.arguments?.getString("cliente")

E a tela foi adaptada para isso:

text = "PEDIDOS - $cliente"

Diferente do perfil, aqui a navegação não depende obrigatoriamente do envio do parâmetro.

3. Envio efetivo do parâmetro opcional

No commit seguinte, a navegação passa a enviar o valor de fato.

Antes:

navController.navigate("pedidos")

Depois:

navController.navigate("pedidos?cliente=Cliente XPTO")

Agora o dado é incorporado à rota, permitindo que a tela exiba informações dinâmicas em vez de depender apenas do valor padrão.

4. Múltiplos parâmetros obrigatórios e tipados

Por fim, a tela de perfil evolui para suportar múltiplos parâmetros obrigatórios com tipagem explícita:

composable(
route = "perfil/{nome}/{idade}",
arguments = listOf(
navArgument("nome") { type = NavType.StringType },
navArgument("idade") { type = NavType.IntType }
)
)

A navegação passa a enviar ambos os valores:

navController.navigate("perfil/Fulano de Tal/27")

E os dados são recuperados com tipagem:

val nome = it.arguments?.getString("nome")
val idade = it.arguments?.getInt("idade")

A tela agora depende explicitamente desses dois parâmetros:

text = "PERFIL - $nome tem $idade anos"



Os commits mostram a transição de telas estáticas para telas orientadas por dados da navegação:

Parâmetros obrigatórios (Perfil)
Parâmetros opcionais (Pedidos)
Uso efetivo dos parâmetros
Múltiplos argumentos com tipagem

Isso torna a navegação mais flexível, reutilizável e estruturada, além de transformar a rota em um mecanismo de transporte de estado entre telas.