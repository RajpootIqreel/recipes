# recipes
import React, { useState } from 'react';
import { View, Text, TextInput, Button, FlatList, TouchableOpacity, StyleSheet } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();

// Recipe data
const RECIPES = [
  { id: '1', name: 'Pasta Carbonara', category: 'Italian', ingredients: 'Pasta, Eggs, Cheese, Bacon', instructions: 'Cook pasta, mix eggs and cheese, combine with bacon.' },
  { id: '2', name: 'Chicken Curry', category: 'Indian', ingredients: 'Chicken, Curry powder, Coconut milk', instructions: 'Cook chicken, add curry powder and coconut milk.' },
  { id: '3', name: 'Sushi', category: 'Japanese', ingredients: 'Rice, Nori, Fish', instructions: 'Cook rice, roll with fish and nori.' },
];

// Home Screen component
function HomeScreen({ navigation }) {
  const [searchQuery, setSearchQuery] = useState('');
  
  // Filter recipes based on search query
  const filteredRecipes = RECIPES.filter(recipe =>
    recipe.name.toLowerCase().includes(searchQuery.toLowerCase()) || 
    recipe.category.toLowerCase().includes(searchQuery.toLowerCase())
  );

  return (
    <View style={styles.container}>
      <TextInput
        style={styles.searchBar}
        placeholder="Search by name or category..."
        value={searchQuery}
        onChangeText={setSearchQuery}
      />
      <FlatList
        data={filteredRecipes}
        keyExtractor={item => item.id}
        renderItem={({ item }) => (
          <TouchableOpacity
            style={styles.recipeItem}
            onPress={() => navigation.navigate('Details', { recipe: item })}
          >
            <Text style={styles.recipeName}>{item.name}</Text>
            <Text style={styles.recipeCategory}>{item.category}</Text>
          </TouchableOpacity>
        )}
      />
    </View>
  );
}

// Details Screen component
function DetailsScreen({ route }) {
  const { recipe } = route.params;

  return (
    <View style={styles.container}>
      <Text style={styles.recipeTitle}>{recipe.name}</Text>
      <Text style={styles.sectionTitle}>Ingredients:</Text>
      <Text>{recipe.ingredients}</Text>
      <Text style={styles.sectionTitle}>Instructions:</Text>
      <Text>{recipe.instructions}</Text>
    </View>
  );
}

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} options={{ title: 'Recipes' }} />
        <Stack.Screen name="Details" component={DetailsScreen} options={{ title: 'Recipe Details' }} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

// Styles
const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  searchBar: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    marginBottom: 20,
    paddingHorizontal: 10,
    borderRadius: 5,
  },
  recipeItem: {
    padding: 15,
    borderBottomWidth: 1,
    borderBottomColor: '#ddd',
  },
  recipeName: {
    fontSize: 18,
    fontWeight: 'bold',
  },
  recipeCategory: {
    fontSize: 14,
    color: 'gray',
  },
  recipeTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  sectionTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    marginTop: 15,
    marginBottom: 5,
  },
});
