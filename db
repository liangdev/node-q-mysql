/*
 usage (how to chain the queries):

 var sql = 'select * from user where id=9';
 var res = db.query(sql);

 res = res.then(function(users) {
    var user = users[0];
    var sql = 'select * from company where id='+user.company_id;
    return db.query(sql);
 });

 res = res.then(function(companies) {
    console.log(companies);
 });

 res = res.catch(function(err) {
    console.log(err);
 });

 res = res.fin(function() {
    console.log('finally out');
    db.conn.end();
 });

 */

var mysql = require('mysql');
var nconf = require('nconf');
nconf.file({ file: '../config/config.json' });
var q = require("q");

exports.db = {

    conn: null,
    createConnection: function(config) {

        var _this = this;

        return q.Promise(function(resolve, reject, notify) {
            if(!config) {
                config = nconf.get('mysql');
            }
            _this.conn = mysql.createConnection(config);
            _this.conn.connect(function(err) {
                if(err) {
                    reject();
                } else {
                    resolve();
                }
            });
        });
    },
    query: function(sql) {
        var _this = this;

        if(!_this.conn) {
            _this.createConnection();
        }

        return q.Promise(function(resolve, reject, notify) {
            _this.conn.query(sql, function(err, rows) {
                if(err) {
                    reject();
                } else {
                    resolve(rows);
                }
            });
        });
    },
    prepare: function(sql, params) {
        return mysql.format(sql, params);
    }
};
