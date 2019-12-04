drop table if exists transport_meaning;
drop table if exists type_transport_meaning;
drop table if exists transport_meaning_class;
drop table if exists manufacturers;
drop table if exists character_attributes_color_species;
drop table if exists character_attributes_color_character;
drop table if exists character;
drop table if exists character_attributes_color;
drop type if exists body_attribute;
drop table if exists species;
drop table if exists language;
drop table if exists designation;
drop table if exists classification;
drop table if exists climate_planets;
drop table if exists terrain_planets;
drop table if exists planets;
drop table if exists terrain;
drop table if exists climate;
drop table if exists gender;


create table gender(
    id serial
                   constraint gender_pk primary key ,
    gender varchar
);

--- planets ---
create table climate(
    id serial
                    constraint climate_pk primary key ,
    climate_type varchar
);

create table terrain(
    id serial
                    constraint terrain_pk primary key ,
    terrain_type varchar
);

create table planets(
    id serial
                    constraint planets_pk primary key ,
    name varchar,
    rotation_period int,
    orbital_period int,
    diameter int,
    gravity_in_standard float,
    surface_water float,
    population double precision
);

create table terrain_planets(
    id_terrain_idx int
                            constraint terrain_fk
                            references terrain,
    id_planet_idx int
                            constraint planet_fk
                            references planets,
    primary key (id_planet_idx,id_terrain_idx)
);

create table climate_planets(
    id_climate_idx int
                            constraint climate_fk
                            references climate,
    id_planet_idx int
                            constraint planet_fk
                            references planets,
    primary key (id_planet_idx,id_climate_idx)
);

--- species ---
create table classification(
    id serial
                           constraint classification_pk primary key ,
    type varchar
);

create table designation(
    id serial
                        constraint designation_pk primary key ,
    designation varchar
);

create table language(
    id serial
                     constraint language_pk primary key ,
    language_name varchar
);

create table species(
    id serial
                    constraint species_pk primary key ,
    name varchar,
    average_height int,
    average_lifespan int,
    classification_idx int
                    constraint classification_fk
                    references classification,
    language_idx int
                    constraint language_fk
                    references language,
    homeworld_idx int
                    constraint homeworld_fk
                    references planets,
    designation_idx int
                    constraint designation_fk
                    references designation
);

create type body_attribute as enum ('skin','hair','eyes');
create table character_attributes_color(
    id serial
                       constraint character_attributes_color_pk primary key ,
    color varchar,
    body_part body_attribute
);


--- character ---

create table character(
    id serial
                      constraint character_pk primary key,
    name varchar,
    height float,
    mass float,
    birth_year varchar,
    homeworld_idx int
                      constraint homeworld
                      references planets,
    gender_idx int
                      constraint gender_fk
                      references gender

);

create table character_attributes_color_character(
    id_character_attributes_color int
                                 constraint character_attributes_color_fk
                                 references character_attributes_color
                                 on delete cascade ,
    id_character int
                                 constraint character_fk
                                 references character
                                 on delete cascade ,
    primary key (id_character,id_character_attributes_color)
);

create table character_attributes_color_species(
    id_character_attributes_color int
                                               constraint character_attributes_color_fk
                                               references character_attributes_color
                                               on delete cascade ,
    id_species int
                                               constraint species_fk
                                               references species
                                               on delete cascade
);


--- vehicules & starships ---

create table manufacturers(
    id serial
                          constraint manufacturers_pk primary key ,
    name varchar
);

create table transport_meaning_class(
    id serial
                                    constraint class_pk primary key ,
    class varchar
);

create table type_transport_meaning(
    id serial
                                   constraint type_pk primary key ,
    type varchar
);


create table transport_meaning(
    id serial
                              constraint transport_meaning_pk primary key ,
    name varchar,
    model varchar,
    cost_in_credits double precision,
    length float,
    max_atmospheric_speed int,
    crew int,
    passengers int,
    cargo_capacity int,
    consumables int,
    hyperdrive_rating float,
    MGTL int,
    manufacturer_idx int
                              constraint manufacturer_fk
                              references manufacturers,
    class_idx int
                              constraint class_fk
                              references transport_meaning_class,
    type_idx int
                              constraint type_fk
                              references type_transport_meaning

);
