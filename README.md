# POO-PHP
BASE DE UM JOGO USANDO O CONTEUDO APRENDIDO EM SALA SOBRE POO NA LINGUAGEM PHP
<?php
class Personagem
{
  public $vida;
  public $nickname;
  public $experiencia;
  public $item;
  public $inventario;


  public function __construct($vida, $nickname, $experiencia, $item, $inventario)
  {
    $this -> vida = $vida;
    $this -> nickname = $nickname;
    $this -> experiencia = $experiencia;
    $this -> item = $item;
    $this -> inventario = [];
  }
    

  public function atacar($inimigo)
  {
    $dano = mt_rand(1,10);
    $inimigo -> vida -= $dano;
  }


  public function Infos()
  {
    echo "Nome: {$this -> nickname} \n";
    echo "Vida: {$this -> vida} \n";
    echo "Experiencia: {$this -> experiencia} \n";
  }


  public function fugir()
  {
    echo "{$this -> nickname} voce fugiu da batalha";
  }


  public function adicionarItem($itens)
  {
    $this -> inventario[] = $itens;
    echo "{$this -> nickname} adquiriu o item: {$itens -> nome}";
  }


  public function usarItem($indice)
{
    if (isset($this->inventario[$indice])) 
    {
        $item = $this->inventario[$indice];
        $item->acao($this);
        unset($this->inventario[$indice]);
        echo "{$this->nickname} usou um item: {$item->nome}\n";
    } 
    
    
    else 
    {
        echo "Item não encontrado no inventário.\n";
    }

}

}


//simulaçao de criaçao de lutador e inimigo
$jogador = new Personagem(100, 'heroi', 0, [],[]);
$inimigo = new Mobs($defesaPorRaridade['inicial'], $valoresDeAtaquePorRaridade['inicial'], 100);


class Armas
{
  public $tipo;
  public $raridade;
  public $dano;


  public function __construct($tipo, $raridade, $dano)
  {
    $this -> tipo = $tipo;
    $this -> raridade = $raridade;
    $this -> dano = $dano;
    
  }
}


$armas = [
  new Armas('Espada de madeira', 'comum', 10),
  new Armas('Espada de ferro', 'raro', 25),
  new Armas('Espada magica', 'epica', 45),
  new Armas('Arco de madeira', 'comum', 10),
  new Armas('Arco de ferro', 'raro', 25),
  new Armas('Arco magico', 'epico', 45),
  new Armas('Cajado comum', 'comum', 10),
  new Armas('Cajado raro', 'raro', 25),
  new Armas('Cajado epico', 'epico', 45),
];


class Item
{
    public $nome;
    public $descricao;
    public $acao;

    public function __construct($nome, $descricao, $acao)
    {
        $this->nome = $nome;
        $this->descricao = $descricao;
        $this->acao = $acao;
    }
}


// Criar alguns itens de cura
$itensCura = [
    new Item('Poção de Cura', 'Recupera 20 pontos de vida', function ($personagem) 
    {
        $personagem->vida += 20;
    }),


    new Item('Poção Superior de Cura', 'Recupera 50 pontos de vida', function ($personagem) 
    {
        $personagem->vida += 50;
    }),
];


class Mobs
{
  public $raridade;
  public $ataque;
  public $defesa;


  public function __construct($raridade, $ataque, $defesa)
  {
    $this -> raridade = $raridade;
    $this -> ataque = $ataque;
    $this -> defesa = $defesa;
   
    
  }


  public function atacar($jogador)
  {
    $dano = mt_rand(1,10);
    $jogador -> vida -= $dano;

  }

}


$defesaPorRaridade = [
  'inicial' => 100,
  'evoluido' => 125,
  'epico' => 200,
  'lendario' => 300,
  'exotico' => 420,

];


$valoresDeAtaquePorRaridade = [
  'inicial' => 10,
  'evoluido' => 20,
  'epico' => 30,
  'lendario' => 45,
  'exotico' => 70,

];


$mobInicial = new Mobs('inicial', $valoresDeAtaquePorRaridade['inicial'], $defesaPorRaridade['inicial'] );
$mobEvoluido = new Mobs('evoluido', $valoresDeAtaquePorRaridade['evoluido'], $defesaPorRaridade['evoluido']);
$mobEpico = new Mobs('epico', $valoresDeAtaquePorRaridade['epico'], $defesaPorRaridade['epico']);
$mobLendario = new Mobs('lendario', $valoresDeAtaquePorRaridade['lendario'], $defesaPorRaridade['lendario'] );
$mobExotico = new Mobs('exotico', $valoresDeAtaquePorRaridade['exotico'], $defesaPorRaridade['exotico']);


function Menu()
{
  echo "Escolha uma ação: \n";
  echo "1. Atacar \n";
  echo "2. Usar Item \n";
  echo "3. Fugir \n";
  echo "4. Mostrar Status \n";

}


while ($jogador->vida > 0 && $inimigo->defesa > 0) 
{
  Menu();
  $escolha = readline("Escolha uma opcao: \n");


  switch ($escolha) {
    case '1':
      $jogador->atacar($inimigo);
      break;


      case '2':
        echo "Escolha um item para usar:\n";
        for ($i = 0; $i < count($jogador->inventario); $i++) {
            $item = $jogador->inventario[$i];
            echo "$i. {$item->nome} - {$item->descricao}\n";
        }
        $indiceItem = readline("Digite o número do item: ");
        $jogador->usarItem($indiceItem);
        break;
    

    case '3':
      $chanceDeFuga = mt_rand(1, 100); 
      if ($chanceDeFuga <= 30) 
      { // Exemplo: 30% de chance de fugir com sucesso
        echo "{$jogador->nickname} fugiu da batalha.\n";
        break;
      } 
      
      
      else 
      {
        echo "{$jogador->nickname} não conseguiu fugir.\n";
      }
      break;


    case '4':
      echo "Mostrando status:\n";
      $jogador->Infos(); 
      break;
    
      
    default:
      echo "Opcao invalida.\n";
      break;
  }

  if ($jogador->vida <= 0)
  {
    echo "{$jogador->nickname} foi derrotado.\n";
  }
  
  
  else
  {
    echo "{$inimigo->raridade} foi derrotado por {$jogador->nickname}.\n";
    echo "Parabéns, {$jogador->nickname}! Você derrotou {$inimigo->raridade}.\n";
  }
}


?>
