# Racha-conta
//App mobile para auxiliar pessoas que frequentam bares a dividir a conta de forma fácil entre os presentes.

import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.indigo,
      ),
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {

  // VARIAVEIS
  final _tValor = TextEditingController();
  final _tQuantidade = TextEditingController();
  final _tGarcom = TextEditingController();
  var _infoText = "Informe seus dados!";//testo fixo
  var _formKey = GlobalKey<FormState>();

  @override //siguinifica q a função procima esta sendo usada em uma classe ancestral
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Racha Conta"),
        centerTitle: true,
        actions: <Widget>[
          IconButton(icon: Icon(Icons.refresh), // icone de atualisar, ao clicar ele limpa campo
              onPressed: _resetFields)
        ],
      ),
      body: _body(),
    );
  }

  // PROCEDIMENTO PARA LIMPAR OS CAMPOS
  void _resetFields(){
    _tValor.text = "";
    _tQuantidade.text = "";
    _tGarcom.text = "";
    setState(() {
      _infoText = "Informe seus dados!";
      _formKey = GlobalKey<FormState>();
    });
  }

  _body() {
    return SingleChildScrollView( // configuarção para que o app não fiqui com os ptsos estorados em tela maior
        padding: EdgeInsets.all(15.0),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: <Widget>[
              _editText("Valor total da conta", _tValor),//testo marca daqua de aparece dentro da onde vai escrever
              _editText("Quantidade de pessoas", _tQuantidade),
              _editText("Porcentagem do garçom", _tGarcom),
              _buttonCalcular(),
              _textInfo(),
            ],
          ),
        ));
  }

  // Widget text
  _editText(String field, TextEditingController controller) {//parte do teclado
    return TextFormField(
      controller: controller,
      validator: (s) => _validate(s, field),
      keyboardType: TextInputType.number,
      style: TextStyle(
        fontSize: 22,
        color: Colors.indigo,
      ),
      decoration: InputDecoration(
        labelText: field,
        labelStyle: TextStyle(
          fontSize: 22,
          color: Colors.grey,
        ),
      ),
    );
  }

  // PROCEDIMENTO PARA VALIDAR OS CAMPOS
  String _validate(String text, String field) {
    if (text.isEmpty) {
      return "Digite $field";
    }
    return null;
  }

  // Widget button
  _buttonCalcular() {//configuração do botao
    return Container(
      margin: EdgeInsets.only(top: 10.0, bottom: 20),
      height: 45,
      child: RaisedButton(
        color: Colors.indigo,
        child:
        Text(
          "Calcular",
          style: TextStyle(
            color: Colors.white,
            fontSize: 22,
          ),
        ),
        onPressed: () {
          if(_formKey.currentState.validate()){
            _calculate();
          }
        },
      ),
    );
  }

  // PROCEDIMENTO PARA CALCULAR O IMC
  void _calculate(){
    setState(() {
      double valor = double.parse(_tValor.text);
      double quantidade = double.parse(_tQuantidade.text);
      double percentual = double.parse(_tGarcom.text)/100;
      double pGarcom = (valor*percentual);
      double pTotal = valor + pGarcom ;
      double pIndividual = pTotal/quantidade;



      String pTotalStr = pTotal.toStringAsPrecision(5);
      String pGarcomStr = pGarcom.toStringAsPrecision(5);
      String pIndividualStr = pIndividual.toStringAsPrecision(4);
      String valorlStr = valor.toStringAsPrecision(5);

        _infoText = "Valor total consumido (R\$$valorlStr)\nTotal da parte garçom (R\$$pGarcomStr)\nTotal para cada pessoa (R\$$pIndividualStr)\n\nValor total a pagar (R\$$pTotalStr)";


    });
  }

  // // Widget text
  _textInfo() {
    return Text(
      _infoText,
      textAlign: TextAlign.justify,
      style: TextStyle(color: Colors.black, fontSize: 25.0),
    );
  }
}
