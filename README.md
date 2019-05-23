# AgendaEmTxt
Agenda toda feita em C# usando txt como armazenamento de dados.

using System;
using System.Collections.Generic;
using System.IO;

namespace AgendaProjec_1
{
    class Program
    {
        //Listas Globais
        static List<string> ListaNome;
        static List<int> ListaIdade;
        static List<char> ListaSexo;
        static List<int> ListaTelefone;
        static List<int> ListaID;

        //Variaveis Globais
        static string Nome;
        static int Idade;
        static char Sexo;
        static int Telefone;
        static int ContadorDeContatos;
        static int ID_Usuario;
        static StreamWriter EscreverArquivo;
        static StreamReader LerArquivo;
        static string CaminhoDaPasta = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments) + @"\Contatos.txt";
        //===================================================================================================================

        //Ler o txt↓
        static void OutputTxt()
        {
            //Conferir se o arquvio exite↓
            if (!File.Exists(CaminhoDaPasta))
            {
                EscreverArquivo = new StreamWriter(CaminhoDaPasta);
                EscreverArquivo.Dispose();
            }
            //-----------------------------------------------------------

            ListaID = new List<int>();
            ListaNome = new List<string>();
            ListaIdade = new List<int>();
            ListaSexo = new List<char>();
            ListaTelefone = new List<int>();

            string[] VetorLinhaArquivo = { };
            int contador = 0;

            //Lendo arquivos no txt↓
            LerArquivo = new StreamReader(CaminhoDaPasta);
            while (!LerArquivo.EndOfStream)
            {
                string linhaArquivo = LerArquivo.ReadLine();
                if (linhaArquivo.Contains("-"))
                {
                    ListaID.Add(contador);
                    contador++;
                }

                if (linhaArquivo.Contains(':'))
                {
                    VetorLinhaArquivo = linhaArquivo.Split(':');
                }

                if (linhaArquivo.Contains("Nome"))
                {
                    ListaNome.Add(VetorLinhaArquivo[1].Trim());
                }

                if (linhaArquivo.Contains("Idade"))
                {
                    ListaIdade.Add(int.Parse(VetorLinhaArquivo[1].Trim()));
                }

                if (linhaArquivo.Contains("Sexo"))
                {
                    ListaSexo.Add(char.Parse(VetorLinhaArquivo[1].Trim()));
                }
                if (linhaArquivo.Contains("Fone"))
                {
                    ListaTelefone.Add(int.Parse(VetorLinhaArquivo[1].Trim()));
                }
            }

            LerArquivo.Dispose();
        }

        //Escreve no txt↓
        static void InputTxt()
        {
            //Escrevendo no arquivo txt↓
            EscreverArquivo = new StreamWriter(CaminhoDaPasta, false);
            Console.Clear();
            for (int i = 0; i < ListaID.Count; i++)
            {
                EscreverArquivo.WriteLine("------------------------");
                EscreverArquivo.WriteLine("Nome: " + ListaNome[i]);
                EscreverArquivo.WriteLine("Idade: " + ListaIdade[i]);
                EscreverArquivo.WriteLine("Sexo: " + ListaSexo[i]);
                EscreverArquivo.WriteLine("Fone: " + ListaTelefone[i]);
            }
            EscreverArquivo.Dispose();

        }

        //04↓
        static void Pesquisar()
        {
            Console.Clear();

            //Cabeçalho
            Console.WriteLine("\t\tPRECIONE PARA CONTINUAR");
            Console.ForegroundColor = ConsoleColor.DarkMagenta;       
            Console.WriteLine("\t   *********************************");
            Console.ForegroundColor = ConsoleColor.White;
            //**********************************************

            LerArquivo = new StreamReader(CaminhoDaPasta);
            while (!LerArquivo.EndOfStream)
            {
                Console.WriteLine(LerArquivo.ReadLine());
            }

            LerArquivo.Dispose();

            
            Console.ReadKey();
        }

        //03↓
        static void Remover()
        {
            Console.Clear();
            int ID_Usuario;
            //Cabeçalho
            Console.WriteLine("\t\tPrecione XX para retornar ao MENU");
            Console.ForegroundColor = ConsoleColor.DarkMagenta;
            Console.WriteLine("\t\t---------------------------------");
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("\t\t         Remova o Contato");
            Console.ForegroundColor = ConsoleColor.White;
            //**********************************************

            Console.Write("\tNome do Usuario: ");
            Nome = Console.ReadLine().ToUpper();
            if (Nome == "XX")
            {
                Menu();
            }

            for (int i = 0; i < ListaNome.Count; i++)
            {
                if (Nome == ListaNome[i])
                {
                    Console.WriteLine("\tContato");
                    Console.WriteLine("ID do Usuario: " + ListaID[i]);
                    Console.WriteLine("Nome: " + ListaNome[i]);
                    Console.WriteLine("Idade: " + ListaIdade[i]);
                    Console.WriteLine("Sexo: " + ListaSexo[i]);
                    Console.WriteLine("Telefone: " + ListaTelefone[i]);
                }
            }
            Console.Write("\t\nID do Contato: ");
            {
                ID_Usuario = int.Parse(Console.ReadLine());
                try
                {
                    if (ID_Usuario < 0)
                    { Console.WriteLine("Numeros negativos NÃO são permitidos"); }

                }
                catch
                {
                    Console.WriteLine("Opção não permitida");
                    Console.ReadKey();
                }
            }

            ListaID.RemoveAt(ID_Usuario);
            ListaNome.RemoveAt(ID_Usuario);
            ListaIdade.RemoveAt(ID_Usuario);
            ListaSexo.RemoveAt(ID_Usuario);
            ListaTelefone.RemoveAt(ID_Usuario);

            InputTxt();
        }

        //02↓
        static void Editar()
        {
            Console.Clear();
            //Cabeçalho
            Console.WriteLine("\t\tPrecione XX para retornar ao MENU");
            Console.ForegroundColor = ConsoleColor.DarkMagenta;
            Console.WriteLine("\t\t---------------------------------");
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("\t\t          Edite o Contato");
            Console.ForegroundColor = ConsoleColor.White;
            //**********************************************

            foreach (string item in ListaNome)
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine(item);
                Console.ForegroundColor = ConsoleColor.White;
            }

            Console.Write("\nNome do Usuario: ");
            Nome = Console.ReadLine().ToUpper();
            if (Nome == "XX")
            {
                Menu();
            }
            
            for (int i = 0; i < ListaNome.Count; i++)
            {
                if (Nome == ListaNome[i])
                {
                    Console.ForegroundColor = ConsoleColor.Magenta;
                    Console.WriteLine("---------------------------------");
                    Console.ForegroundColor = ConsoleColor.White;

                    Console.WriteLine("Contato");
                    Console.WriteLine("ID: " + ListaID[i]);
                    Console.WriteLine("Nome: " + ListaNome[i]);
                    Console.WriteLine("Idade: " + ListaIdade[i]);
                    Console.WriteLine("Sexo: " + ListaSexo[i]);
                    Console.WriteLine("Telefone: " + ListaTelefone[i]);

                    Console.ForegroundColor = ConsoleColor.Magenta;
                    Console.WriteLine("---------------------------------");
                    Console.ForegroundColor = ConsoleColor.White;
                }
            }
            Console.WriteLine("Informe o ID do contato que deseja Editar.");
            {
                ID_Usuario = int.Parse(Console.ReadLine());
                try
                {
                    if (ID_Usuario < 0)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Numeros negativos NÃO são permitidos");
                        Console.ReadKey(); Console.Clear();
                        Editar();
                    }
                }
                catch
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("Opção não permitida");
                    Console.ReadKey(); Console.Clear();
                    Editar();
                }
            }
            ListaID.RemoveAt(ID_Usuario);
            ListaNome.RemoveAt(ID_Usuario);
            ListaIdade.RemoveAt(ID_Usuario);
            ListaSexo.RemoveAt(ID_Usuario);
            ListaTelefone.RemoveAt(ID_Usuario);

            Console.Clear();
            NovoContato();
        }
        //02.1↓
        static void NovoContato()
        {
            //Contato Editado
            Console.WriteLine("\t\tPrecione XX para retornar ao MENU");
            Console.ForegroundColor = ConsoleColor.DarkMagenta;
            Console.WriteLine("\t\t---------------------------------");
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("\t\tNovos Dados do contato");
            Console.ForegroundColor = ConsoleColor.White;
            //**********************************************

            Console.Write("Nome: ");
            Nome = Console.ReadLine().ToUpper();
            if (Nome == "XX")
            {
                Menu();
            }

            try
            {
                Console.Write("Idade: ");
                Idade = int.Parse(Console.ReadLine());
                if (Idade < 0)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("Numeros negativos NÃO são permitidos");
                    Console.ForegroundColor = ConsoleColor.White;
                    Console.ReadKey(); Console.Clear();
                    NovoContato();
                }

                Console.Write("Sexo: ");
                Sexo = Console.ReadLine().Trim().ToUpper()[0];
                if (Sexo != 'M' && Sexo != 'F')
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("Opção Invalida, favor inserir o sexo corretamente");
                    Console.ReadKey(); Console.Clear();
                    NovoContato();
                }

                Console.Write("Telefone: ");
                Telefone = int.Parse(Console.ReadLine());
                if (Telefone < 0)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("Numeros negativos NÃO são permitidos");
                    Console.ReadKey(); Console.Clear();
                    NovoContato();
                }
            }
            catch
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Opção não permitida");
                Console.ReadKey(); Console.Clear();
                NovoContato();
            }

            ListaID.Insert(ID_Usuario, ContadorDeContatos);
            ListaNome.Insert(ID_Usuario, Nome);
            ListaIdade.Insert(ID_Usuario, Idade);
            ListaSexo.Insert(ID_Usuario, Sexo);
            ListaTelefone.Insert(ID_Usuario, Telefone);
            InputTxt();
        }

        //01↓
        static void Adicionar()
        {
            Console.Clear();
           
            char novoContato = 'S';
            while (novoContato == 'S')
            {
                //Cabeçalho
                Console.WriteLine("\t\tPrecione XX para retornar ao MENU");
                Console.ForegroundColor = ConsoleColor.DarkMagenta;
                Console.WriteLine("\t\t---------------------------------");
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("\t\t        Adicione o Contato");
                Console.ForegroundColor = ConsoleColor.White;
                //**********************************************

                Console.Write("Nome: ");
                Nome = Console.ReadLine().ToUpper();
                if (Nome == "XX")
                {
                    break;
                }

                try
                {
                    Console.Write("Idade: ");
                    Idade = int.Parse(Console.ReadLine());
                    if (Idade < 0)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Numeros negativos NÃO são permitidos");
                        Console.ReadKey(); Console.Clear();
                        continue;
                    }

                    Console.Write("Sexo: ");
                    Sexo = Console.ReadLine().Trim().ToUpper()[0];
                    if (Sexo != 'M' && Sexo != 'F')
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Opção Invalida, favor inserir o sexo corretamente");
                        Console.ReadKey(); Console.Clear();
                        continue;
                    }

                    Console.Write("Telefone: ");
                    Telefone = int.Parse(Console.ReadLine());
                    if (Telefone < 0)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Numeros negativos NÃO são permitidos");
                        Console.ReadKey(); Console.Clear();
                        continue;
                    }
                }
                catch
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("Opção não permitida");  
                    Console.ReadKey(); Console.Clear();
                    continue;
                }
                Console.ForegroundColor = ConsoleColor.DarkCyan;
                Console.WriteLine("Deseja Adicionar mais algum contato? (S|N)");
                Console.ForegroundColor = ConsoleColor.White;
                novoContato = char.Parse(Console.ReadLine().ToUpper());
                if (novoContato != 'S' && novoContato != 'N')
                {
                    Console.ForegroundColor = ConsoleColor.DarkRed;
                    Console.WriteLine("Opção Invalida, permitido Apenas S para Sim e N para NÃO");
                    Console.ForegroundColor = ConsoleColor.Cyan;
                    Console.WriteLine("PRECIONE PARA CONTINUAR");
                    Console.ReadKey();
                    Console.Clear();
                    continue;

                }

                ListaID.Add(ContadorDeContatos);
                ListaNome.Add(Nome);
                ListaIdade.Add(Idade);
                ListaSexo.Add(Sexo);
                ListaTelefone.Add(Telefone);

                ContadorDeContatos++;
                InputTxt();
            }
        }

        //Chama todos eles↑
        static void Menu()
        {
            OutputTxt();

            int OpcaoMenu = 0;
            bool rodar = true;

            while (rodar)
            {
                Console.Clear();
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine("1 - Adicionar | 2 - Editar | 3 - Remover | 4 - Pesquisar | 0 - Finalizar");
                Console.ForegroundColor = ConsoleColor.White;
                try
                {
                    OpcaoMenu = int.Parse(Console.ReadLine());
                    if (OpcaoMenu < 0)
                    {
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Numero negativo não são permitidos");
                        Console.ReadKey(); Console.Clear();
                        continue; ;
                    }
                }
                catch
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("Caracter não permitido");
                    Console.ReadKey(); Console.Clear();
                    continue;
                }

                switch (OpcaoMenu)
                {
                    case 1: Adicionar(); continue;
                    case 2: Editar(); continue;
                    case 3: Remover(); continue;
                    case 4: Pesquisar(); continue;
                    case 0: rodar = false; continue;
                    case 11: 

                    default:
                        Console.ForegroundColor = ConsoleColor.Red;
                        Console.WriteLine("Opção Invalida");
                        Console.ReadKey(); Console.Clear();
                        continue;
                }
            }
        }

        //Main↓
        static void Main(string[] args)
        {
            Menu();
        }
    }
}
