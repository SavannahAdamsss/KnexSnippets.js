// Knex Snippets:


// Ruby snippet:

const environment = process.env.NODE_ENV || 'development'
const knexConfig = require('./knexfile')[environment]
const knex = require('knex')(knexConfig)
module.exports = knex





// Objective-C snippet:

module.exports = {
  development: {
    client: 'pg',
    connection: 'postgres://localhost/dbname-dev'
  },
  test: {},
  production: {
    client: 'pg',
    connection: process.env.DATABASE_URL
  }
}






//JS/ JSON Snippet:

// CRUD commands...

// LIST  select * from allergies

knex('allergies')
  .then(data => {
    console.log("SELECT", data)
  })
  .catch(err => {
    console.log("BAD THING...", err)
  })
  
  
  

// // CREATE
knex('allergies')
  .insert([{ allergen: 'grasses', pollen_count: 5},
    {allergen: 'more grasses', pollen_count: 10}])
  .returning('*')
  .then(data => {
    console.log("RETURN FROM INSERT", data)
  })






// READ
// let userId = '; drop table allergies;' <- CLASSIC SQL INJECTION!!
let userId = 8;

knex('allergies')
  // .select('id', 'allergen')
  .where('id', +(userId))
  .then(data => {
    console.log("SELECT WITH WHERE CLAUSE", data)
  })




// // UPDATE
knex('allergies')
  .update({ allergen: 'grasses'})
  .where('id', 8)
  .then(data => console.log("RETURN FROM UPDATE", data))





// DELETE
knex('allergies')
  .delete()
  .where('id', 7)
  .returning('*')
  .then(data => console.log("RETURN FROM DELETE", data))
  
  
  
  
  
  
// PHP Migration Boiler Plate: 

exports.up = function(knex, Promise) {
  return knex.schema.createTable('tablename', function(table) {
    // TABLE COLUMN DEFINITIONS HERE
    table.increments()
    table.string('colname1', 255).notNullable().defaultTo('')
    table.string('colname2', 255).notNullable().defaultTo('')
    table.string('colname3', 255).notNullable().defaultTo('')
    table.timestamps(true, true)
    // OR
    // table.dateTime('created_at').notNullable().defaultTo(knex.raw('now()'))
    // table.dateTime('updated_at').notNullable().defaultTo(knex.raw('now()'))
  })
}
exports.down = function(knex, Promise) {
  return knex.schema.dropTableIfExists('tablename')
}




// PHP Snippet KNEX SEED:

exports.seed = function(knex, Promise) {
  // Deletes ALL existing entries
  return knex('allergies').del()
    .then(function () {
      // Inserts seed entries
      return knex('allergies').insert([
        {id: 1, allergen: 'flower', pollen_count: 3000},
        {id: 2, allergen: 'pine', pollen_count: 10}
      ]);
    })
    .then(function() {
      // Moves id column (PK) auto-incrementer to correct value after inserts
      return knex.raw("SELECT setval('allergies_id_seq', (SELECT MAX(id) FROM allergies))")
    })
};




// Knex Update Route JS / JSON:

/* UPDATE */
router.patch('/:id', function(req, res, next) {

  let { allergen, pollen_count } = req.body

  let updateInfo = {}
  if (allergen !== undefined) updateInfo.allergen = allergen
  if (pollen_count !== undefined) updateInfo.pollen_count = +(pollen_count)

  knex('allergies').update(updateInfo).returning('*').then(data =>
    res.send(data[0])
  )

});




// Alergies CRUD routes:

var express = require('express');
var router = express.Router();

const knex = require('../knex')

/* LIST */
router.get('/', function(req, res, next) {
  knex('allergies').then(data =>
    res.send(data)
  ).catch(err => next(err))
});

/* CREATE */
router.post('/', function(req, res, next) {
  let { allergen, pollen_count } = req.body

  knex('allergies').insert({
    allergen,
    pollen_count
  }).returning('*').then(data =>
    res.send(data[0])
  )
});

/* READ */
router.get('/:id', function(req, res, next) {
  knex('allergies')
    .where('id', req.params.id)
    .first()
    .then(data =>
      res.send(data)
    )
    .catch(err => next(err))
});


/* UPDATE */
router.patch('/:id', function(req, res, next) {

  let { allergen, pollen_count } = req.body

  let updateInfo = {}
  if (allergen !== undefined) updateInfo.allergen = allergen
  if (pollen_count !== undefined) updateInfo.pollen_count = +(pollen_count)

  knex('allergies').update(updateInfo).returning('*').then(data =>
    res.send(data[0])
  )
});

/* DELETE */
router.delete('/:id', function(req, res, next) {
  knex('allergies')
    .delete()
    .where('id', req.params.id)
    .then(data =>
      res.send({ message: data + ' records deleted'})
    )
    .catch(err => next(err))
});

module.exports = router;




// Express CRUD matrix practice: Perl snippet:

HTTP method
HTTP path
request body
express route
knex
raw sql
response body 

... VERSUS ...

CRUDL (create, read, update, delete, list)
