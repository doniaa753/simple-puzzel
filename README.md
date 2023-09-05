import 'package:flutter/material.dart';
import 'package:google_fonts/google_fonts.dart';
import 'package:sssss/Home_Screens/HomeScreen.dart';
import 'package:sssss/Home_Screens/Room_Screen.dart';

main() {
  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  PageController _controller = PageController(initialPage: 0);
  int currentQ = 0, point = 0;
  List questions = [
    {
      "question": "What is the capital of Egypt",
      "answer": "Cairo",
      "options": ["Paris", "Cairo", "London", "Madrid"],
    },
    {
      "question": "What is the capital of France",
      "answer": "Paris",
      "options": ["Paris", "Egypt", "London", "Madrid"],
    },
    {
      "question": "What is the capital of UK",
      "answer": "London",
      "options": ["Paris", "Cairo", "London", "Madrid"],
    },
    {
      "question": "What is the capital of Spain",
      "answer": "Madrid",
      "options": ["Paris", "Egypt", "London", "Madrid"],
    },
  ];
  @override
  void initState() {
    //questions.shuffle();
    super.initState();
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        debugShowCheckedModeBanner: false,
        home: Scaffold(
          appBar: AppBar(
              title: Text('Question $currentQ'),
              centerTitle: true,
              actions: [
                Text('$point'),
              ]),
          body: PageView.builder(
              itemCount: questions.length,
              controller: _controller,
              itemBuilder: (context, index) {
                List options = questions[index]['options'];
                options.shuffle();
                print((questions[index]['question']));
                return Column(
                  children: [
                    Center(
                      child: Card(
                        child: Padding(
                          padding: const EdgeInsets.all(60),
                          child: Text(
                            questions[index]['question'],
                            style: TextStyle(
                                fontSize: 30, fontWeight: FontWeight.w600),
                          ),
                        ),
                      ),
                    ),
                    Expanded(
                      child: Card(
                        child: GridView.builder(
                            gridDelegate:
                                const SliverGridDelegateWithFixedCrossAxisCount(
                                    crossAxisCount: 2),
                            itemCount: options.length,
                            itemBuilder: (context, i) {
                              return InkWell(
                                onTap: () {
                                  if (index + 1 == questions.length) {
                                    if (options[i] ==
                                        questions[index]['answer']) {
                                      point += 1;
                                    }

                                    showDialog(
                                        context: context,
                                        builder: (context) {
                                          return AlertDialog(
                                            title: Text('Result '),
                                            content: Text(
                                                '$point / ${questions.length}'),
                                            actions: [
                                              ElevatedButton(
                                                  onPressed: () {
                                                    Navigator.of(context).pop();
                                                    setState(() {
                                                      point = 0;
                                                      currentQ = 0;
                                                      questions.shuffle();
                                                      _controller.jumpTo(0);
                                                    });
                                                  },
                                                  child: Text('Retry')),
                                            ],
                                          );
                                        });
                                  } else {
                                    if (options[i] ==
                                        questions[index]['answer']) {
                                      point += 1;
                                    }

                                    setState(() {
                                      currentQ += 1;
                                    });
                                    _controller.jumpToPage(index + 1);
                                  }
                                },
                                child: Card(
                                  child: Center(
                                      child: Text(
                                    options[i],
                                    style: TextStyle(
                                        fontWeight: FontWeight.w500,
                                        fontSize: 20),
                                  )),
                                ),
                              );
                            }),
                      ),
                    )
                  ],
                );
              }),
        ));
  }
}
# simple-puzzel
create simple puzzel using flutter
