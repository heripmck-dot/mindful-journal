# Mindful Journal

A secure, serene daily journaling application featuring AI-powered reflections to support your mental well-being. Built with React, Tailwind CSS, Supabase, and Google Gemini API.

## Features

- **Secure Authentication**: Email/Password login and signup via Supabase Auth.
- **Cloud Storage**: Journal entries are securely stored in a Postgres database via Supabase.
- **AI Reflections**: Get instant, compassionate feedback on your journal entries using Google's Gemini API.
- **Real-time Sync**: Updates reflect instantly across all open devices.
- **Responsive UI**: A polished, calming interface built with Tailwind CSS.

## Tech Stack

- **Frontend**: React, TypeScript, Tailwind CSS
- **Backend**: Supabase (Auth, Database, Realtime)
- **AI**: Google Gemini API (@google/genai)
- **Icons**: Lucide React

## Setup & Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/mindful-journal.git
   ```

2. **Supabase Setup**
   - Create a new project at [supabase.com](https://supabase.com).
   - Go to the SQL Editor and run the following script to create the necessary table and security policies:

   ```sql
   create table journal_entries (
     id uuid default gen_random_uuid() primary key,
     user_id uuid references auth.users not null,
     title text,
     content text,
     mood text,
     ai_reflection text,
     created_at timestamptz default now(),
     updated_at timestamptz default now()
   );

   alter table journal_entries enable row level security;

   create policy "Users can view their own entries" on journal_entries
     for select using (auth.uid() = user_id);

   create policy "Users can insert their own entries" on journal_entries
     for insert with check (auth.uid() = user_id);

   create policy "Users can update their own entries" on journal_entries
     for update using (auth.uid() = user_id);

   create policy "Users can delete their own entries" on journal_entries
     for delete using (auth.uid() = user_id);
   ```

3. **Running the App**
   - This project uses ES Modules directly in the browser via `index.html`.
   - Ensure you have a valid Supabase project configured in `services/supabaseClient.ts`.
   - Ensure the `API_KEY` environment variable is set for the Gemini API.

