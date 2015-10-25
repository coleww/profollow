profollow
==================

Makes a Twitter account follow back everyone that is following it. A fork of [jimkang/quidprofollow](https://github.com/jimkang/quidprofollow) but with less "quid pro" and more just "pro". 

Often times I will have my bots follow each other because then they will reply or interact in some way. Usually this works out fine with quidprofollow, but if 2 quidprofollow bots are trying to follow one another it can lead to disaster if they are not activated in the correct order. profollow erases all these troubles by making yr bot gleefully follow back everyone. It also might be useful if the goal of yr bot is explicitly to follow lots of spambots, or if yr bot depends on certain users who will never follow you back being in it's timeline. These examples are quite vague, but I assure you that it was totally worth the 3 seconds it took to make this module.

- Q: wait, couldn't u just use the unfollowFilter attribute on quidprofollow and call `done([])`
- A: yes, why yes i just realized that now, thank you.

Installation
------------

    npm install profollow

Usage
-----

    var profollow = require('profollow');
    
    profollow(
      {
        twitterAPIKeys: {
          consumer_key: 'asdfkljqwerjasdfalpsdfjas',
          consumer_secret: 'asdfasdjfbkjqwhbefubvskjhfbgasdjfhgaksjdhfgaksdxvc',
          access_token: '9999999999-zxcvkljhpoiuqwerkjhmnb,mnzxcvasdklfhwer',
          access_token_secret: 'opoijkljsadfbzxcnvkmokwertlknfgmoskdfgossodrh'
        }
      },
      function done(error, followed, unfollowed) {
        console.log('Followed:', followed);
        console.log('Unfollowed:', unfollowed);
      }
    );

Output:

    Followed: [807322189, 17400671, 2911212198]
    Unfollowed: [74496732, 1000904942]

You can also pass a `followFilter` in the opts, which should be a function that takes userIds and calls back with the ids that it is OK to follow.

    quidprofollow(
      {
        twitterAPIKeys: config,
        followFilter: function filterUserIdsHigherThan100Million(userIds, done) {
          var okIds = userIds.filter(function isOver100Million(userId) {
            return userId > 100000000;
          });
          callNextTick(done, null, okIds);
        },
        function done(error, followed, unfollowed, filteredOut) {
          console.log('Followed:', followed);
          console.log('Unfollowed:', unfollowed);
          console.log('Filtered out:', filteredOut);
        }
    );

Tests
-----

Run tests with `make test`.

Run functional test by creating a `config.js` (be careful not to check that in anywhere) in the root directory that looks like this:

    module.exports = {
      twitter: {
        consumer_key: 'asdfkljqwerjasdfalpsdfjas',
        consumer_secret: 'asdfasdjfbkjqwhbefubvskjhfbgasdjfhgaksjdhfgaksdxvc',
        access_token: '9999999999-zxcvkljhpoiuqwerkjhmnb,mnzxcvasdklfhwer',
        access_token_secret: 'opoijkljsadfbzxcnvkmokwertlknfgmoskdfgossodrh'
      }
    };

Then run, `make test-functional`. In this test, you cannot rely on the assertions to make sure things are working because result depend on the followers and followees of the account at the time you run the test. So, check the console output and make sure it's reasonable. It should looks something like:

    Would have followed: { id: 319086998 }
    Would have followed: { id: 607793 }
    Would have followed: { id: 903200748 }
    Would have followed: { id: 114240739 }
    Would have followed: { id: 372519518 }
    Would have followed: { id: 24047669 }
    Would have followed: { id: 37587142 }
    Would have followed: { id: 2292747043 }
    Would have followed: { id: 2509547241 }
    Would have followed: { id: 287422826 }
    Would have unfollowed: { id: 14375294 }

License
-------

MIT.
