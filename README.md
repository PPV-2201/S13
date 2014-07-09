<?

class Element{
  
   public $type;//Тип элемента
   public $pos_children = 0;//счетчик для потомков элемента
   public $pos_attribute = 0;//счетчик для аттрибутов элемента
   public $inner_text;//Внутренний текст элемента
   public $is_inner_text = false;//Имеет ли элемент внутренний текст
   
   public $attributes = array();//Массив аттрибутов
   
   public $childrens = array();//массив потомков
   
   public function add_inner_text($in_txt){
    
    $this->inner_text = $in_txt;
    $this->is_inner_text = true;
    
   }//добавление внутреннего содержимого элемента
   
   public function show_class(){
    
        echo "<$this->type";
        
        if($this->pos_attribute == 0){ echo "\n";}
        
        if($this->pos_attribute != 0){//если есть аттрибуты у текущего элемента - то выводим их
            
            foreach($this->attributes as $atr){
                
                echo " $atr ";
                
            }//foreach
            
        }//if
        
        //Закончил вывод названия элемента и его аттрибутов
         
        echo ">\n";
        
        if($this->is_inner_text){//Если элемент имеет внутренний текст 
            
            echo "\n" . $this->inner_text . "\n";
            
        }//if
        
            
        if($this->pos_children != 0){//если есть потомки у текущего элемента - то вызываем функцию по новой
            
            foreach($this->childrens as $c){
                echo "\n";
                $c->show_class();
                
            }//foreach
            
        }//if
        
        echo "</$this->type>" . "\n";        
    
   }//распечатать дерево элементов
   
   public function add_child(Element $el){
    
    $this->childrens[$this->pos_children] = $el;
    $this->pos_children++;
    
   }//Добавить потомка
    
   public function add_attribute($attr){
    
    $this->attributes[$this->pos_attribute] = $attr;
    $this->pos_attribute ++;
    
   }//Добавить аттрибут
   
   function __construct($TYPE){
    
    $this->type = $TYPE;
    
   }//Конструктор с параметром    
}

$html = new Element('html');
$head = new Element('head');
$meta = new Element('meta');
$meta->add_attribute('charset="utf-8"');
$title = new Element('title');
$title->add_inner_text('PHP - the beast script language!');

$body = new Element('body');
$H1 = new Element('H1');
$H1->add_inner_text('TITLE WHITH H1!!!');

$p = new Element('p');
$p->add_attribute('text-aligment="center"');

//Начинаем сбор html-документа

//Формируем тело(body)

$body->add_child($H1);
$body->add_child($p);

//Собираем голову(head)

$head->add_child($meta);
$head->add_child($title);

//Собираем сам документ

$html->add_child($head);
$html->add_child($body);

$html->show_class($html);
