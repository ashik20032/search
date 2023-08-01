import 'package:flutter/material.dart';
import 'package:cloud_firestore/cloud_firestore.dart';

class Search_Result extends StatelessWidget {
  var Result;

 Search_Result({
    required this.Result,
  });

  final CollectionReference SearchList =
      FirebaseFirestore.instance.collection("Search");

  @override
  Widget build(BuildContext context) {
    return StreamBuilder(
      stream: SearchList.snapshots(),
      builder: (context, AsyncSnapshot<QuerySnapshot> snapshot) {
        if (snapshot.hasData) {
          return ListView.builder(
            itemCount: snapshot.data!.docs.length,
            itemBuilder: (context, index) {
              final DocumentSnapshot SearchData = snapshot.data!.docs[index];

              if (Result.isEmpty) {
                return Container();
              } else if (SearchData["Title"]
                  .toString()
                  .toLowerCase()
                  .startsWith(Result.toString().toLowerCase())) {
                return ListTile(
                  title: SearchData["Title"],
                  subtitle: SearchData["SubTitle"],
                  leading: SearchData["Image"],
                );
              } else if (SearchData["SubTitle"]
                  .toString()
                  .toLowerCase()
                  .startsWith(Result.toString().toLowerCase())) {
                return ListTile(
                  title: SearchData["Title"],
                  subtitle: SearchData["SubTitle"],
                  leading: SearchData["Image"],
                );
              } else if (SearchData["search_tag"]
                  .toString()
                  .toLowerCase()
                  .startsWith(Result.toString().toLowerCase())) {
                return ListTile(
                  title: SearchData["Title"],
                  subtitle: SearchData["SubTitle"],
                  leading: SearchData["Image"],
                );
              } else {
                return Container();
              }
            },
          );
        } else {
          return Container(
              child: Center(
                  child: CircularProgressIndicator(
            color: Color(0xff17203A),
          )));
        }
      },
    );
  }
}
