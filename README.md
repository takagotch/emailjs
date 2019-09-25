### emailjs
---
https://github.com/eleith/emailjs

```js
// test/authssl.js
describe('authorize ssl', function() {
  const {} = require();
  const {} = require();
  const {} = require();
  const email = require();
  const port = 2526;
  
  let server = null;
  let smtp = null;
  
  const send = function(message, verify, done) {
    smtp.onData = function(stream, session, callback) {
      parser(stream)
        .then(verify)
        .then(done)
        .catch(done);
      stream.on('end', callback);
    };
    
    server.send(message, function(err) {
      if (err) {
        throw err;
      }
    });
  };
  
  before(function(done) {
    process.env.NODE_TLS_REJECT_UNAUTHORIZED = '0';
    
    smtp = new stmpServer({ secure: true, authMethods: ['LOGIN'] });
    smtp.listen(port, function() {
      smtp.onAuth = function(auth, session, callback) {
        if (auth.username == 'pooh' && auth.password == 'honey') {
          callback(null, { user: 'pooh' });
        } else {
          return callback(new Error('invalid user / pass'));
        }
      };
      
      server = email.server.connect({
        port: port,
        user: 'pooh',
        password: 'honey',
        ssl: true,
      });
      done();
    });
  });
  
  after(function(done) {
    smtp.close(done);
  });
  
  it('login', function(done) {
    const message = {
      subject: 'this is a test TEXT message from emailjs',
      from: 'pooh@gmail.com',
      to: 'rabbit@gmail.com',
      text: 'hello friend, i hope this message finds you well.',
    };
    
    send(
      email.message.create(message),
      function(mail) {
        expect(mail.text).to.equal(message.text + '\n\n\n');
        expect(mail.subject).to.equal(message.subject);
        expect(mail.from.text).to.equal(message.from);
        expect(mail.to.text).to.equal(message.to);
      },
      done
    );
  });
});
```

```
```

```
```


