Como estava antes
O sistema exibia cada item da lista de lancamentos utilizando um componente padrao que agrupava todas as informacoes em uma unica linha de texto. Os dados de data, descricao, valor e status apareciam concatenados, sem diferenciacao visual entre receitas e despesas.

Codigo anterior
@Composable
private fun List(
    modifier: Modifier = Modifier,
    lancamentos: List<Lancamento>,
    onLancamentoPressed: (Lancamento) -> Unit
) {
    LazyColumn(modifier = modifier) {
        items(lancamentos) { lancamento ->
            val pago = if (lancamento.paga) "pago" else "pendente"
            val descricao = "${lancamento.data.formatar()} - ${lancamento.descricao} - ${lancamento.valor} - $pago"

            ListItem(
                modifier = Modifier.clickable { onLancamentoPressed(lancamento) },
                headlineContent = { Text(descricao) },
            )
        }
    }
}

Como ficou depois
O layout foi modernizado para melhorar a experiencia do usuario. Agora cada item apresenta um icone colorido a esquerda que indica se o lancamento foi pago ou esta pendente. A descricao do lancamento ganhou destaque em uma linha exclusiva. Na segunda linha, a data fica alinhada a esquerda e o valor formatado aparece a direita. Foi implementada uma logica de cores onde o vermelho representa despesas e o verde representa receitas, alem da inclusao do sinal negativo para valores de saida.

Codigo alterado
@Composable
private fun List(
    modifier: Modifier = Modifier,
    lancamentos: List<Lancamento>,
    onLancamentoPressed: (Lancamento) -> Unit
) {
    LazyColumn(modifier = modifier) {
        items(lancamentos) { lancamento ->
            val color = if (lancamento.tipo == TipoLancamentoEnum.DESPESA) Color(0xFFCF5355) else Color(0xFF00984E)
            val icon = if (lancamento.paga) Icons.Filled.ThumbUp else Icons.Filled.ThumbDownOffAlt
            val valorFormatado = if (lancamento.tipo == TipoLancamentoEnum.DESPESA) "-${lancamento.valor.formatar()}" else lancamento.valor.formatar()

            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .clickable { onLancamentoPressed(lancamento) }
                    .padding(16.dp),
                verticalAlignment = Alignment.CenterVertically
            ) {
                Icon(
                    imageVector = icon,
                    contentDescription = null,
                    tint = color,
                    modifier = Modifier.size(40.dp)
                )
                Spacer(modifier = Modifier.width(16.dp))
                Column(modifier = Modifier.weight(1f)) {
                    Text(
                        text = lancamento.descricao,
                        style = MaterialTheme.typography.titleMedium
                    )
                    Row(
                        modifier = Modifier.fillMaxWidth(),
                        horizontalArrangement = Arrangement.SpaceBetween
                    ) {
                        Text(
                            text = lancamento.data.formatar(),
                            style = MaterialTheme.typography.bodySmall
                        )
                        Text(
                            text = valorFormatado,
                            style = MaterialTheme.typography.bodySmall,
                            color = color
                        )
                    }
                }
            }
        }
    }
}
