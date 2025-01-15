SELECT cli.cliente, count(cli.cliente) as qtd FROM SFCUSER.CONTA_CLIENTE cli
inner join sfcuser.cartao car
on car.cliente = cli.cliente
AND car.status = 'C'
AND car.codigo = '73';
group by qtd
